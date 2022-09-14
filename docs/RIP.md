# 🖧 Le protocole RIP

## Présentation

### Définition

???+info "Définition"
    |Protocole RIP||
    |:---------------------:|:-:|
    ```mermaid              
    flowchart TB 
    1{R1} --- 2(R2)
    2 --- 4[(R4)]
    1 --- 3(R3) --- 5
    4 --- 5[/R5\]
    subgraph Protocole RIP
    1
    2
    3
    4
    5
    end
    ```                     
    |Crée en **1988**[^an] Le **Protocole RIP** est un protocole de routage de données prenant en paramètre le **nombre de sauts** entre les appareils pour définir les meilleurs chemins quelque soit la vitesse, la distance ou la qualitée des appareils sur le chemin. |

    Wikipedia[^wiki1]

*[RIP]: Routing Information Protocole
[^an]: [RFC1058](https://www.rfc-editor.org/rfc/rfc1058)
[^wiki1]: [Wikipedia: Routing Information Protocol](https://en.wikipedia.org/wiki/Routing_Information_Protocol)

### Spécificitées techniques

???+TLDR "Spécificitées techniques"
    ???+warning "Spécificitées techniques"
        Ce protocole est **ancien** (1988) il a donc un fonctionnement **très rudimentaire** et **très peu optimisé**.
    
    ???+note "Spécificitées techniques"
        
        |1|2|3|
        |:-:|:-:|:-:|
        |Chaque routeur envoi toutes les 30 secondes la liste des meilleurs routes pour chaque voisin|Ignore la valeur du débit entre chaque routeur|Prend en compte uniquement le nombre de sauts|
        |**4**|**5**|**6**|
        |Ignore la distance entre chaque routeur|Limité a 15 sauts uniquement|A eu 3 versions (RIPv1[^an], RIPv2[^RIP2], RIPng[^RIP3])|
    [^RIP3]: [RFC1723](https://datatracker.ietf.org/doc/html/rfc1723)
    [^RIP2]: [RFC1388](https://datatracker.ietf.org/doc/html/rfc1388)

    ???+info "En-tête RIP"
        ![image](/images/rip.png)
        { align=center }

## Application du protocole

### Cas 1

???+example "Cas 1"
    ???+info "Situation"
        Nous avons un packet de données qui doit aller du routeur **R1** au routeur **R5**. quelle route va emprunter le paquet de données ?
    
    === "Base"
        ???+question "Cas de base"
            ```mermaid              
            flowchart TB 
            1{R1} --- 2(R2)
            2 --- 4(R4)
            1 --- 3(R3) --- 5
            4 --- 5[/R5\]
            2 --- 3
            
            subgraph Réseau
            1
            2
            3
            4
            5
            end

            style 1 fill:#BF0000,stroke:#000,stroke-width:4px,color:#fff
            style 5 fill:#00FF00,stroke:#000,stroke-width:4px,color:#000
            ```   
    === "étape 1"
        ???+question "étape 1"
            ```mermaid              
            flowchart TB 
            1{R1} --- 2(R2)
            2 --- 4(R4)
            1 ===> 3(R3) --- 5
            4 --- 5[/R5\]
            2 --- 3
            
            subgraph Réseau
            1
            2
            3
            4
            5
            end

            style 1 fill:#BF0000,stroke:#000,stroke-width:4px,color:#fff
            style 5 fill:#00FF00,stroke:#000,stroke-width:4px,color:#000
            style 3 fill:#0000FF,stroke:#000,stroke-width:4px,color:#fff
            ``` 
    === "étape 2"
        ???+done "étape 2"
            ```mermaid              
            flowchart TB 
            1{R1} --- 2(R2)
            2 --- 4(R4)
            1 --- 3(R3) ===> 5
            4 --- 5[/R5\]
            2 --- 3
            subgraph Réseau
            1
            2
            3
            4
            5
            end

            style 1 fill:#BF0000,stroke:#000,stroke-width:4px,color:#fff
            style 5 fill:#0000FF,stroke:#000,stroke-width:4px,color:#fff
            ``` 
            > Le paquet n'a pas emprunté `R1 > R2 > R4 > R5` ou `R1 > R2 > R3 > R5` car il y a plus de sauts

### Cas 2

???+example "Cas 2"
    ???+info "Situation"
        Nous avons un packet de données de ^^**2 Giga octets**^^, qui doit aller du routeur **R1** au routeur **R5**. Quelle route va emprunter le paquet de données ?
    
    === "Base"
        ???+question "Cas de base"
            ```mermaid              
            flowchart TB 
            1{R1} ---|1Gbits/s| 2(R2)
            2 --- |1Gbits/s|4(R4)
            1 --- |1Mbits/s|3(R3) ---|1Gbits/s| 5
            4 ---|1Gbits/s| 5[/R5\]
            2 ---|1Gbits/s| 3
            
            subgraph Réseau
            1
            2
            3
            4
            5
            end

            style 1 fill:#BF0000,stroke:#000,stroke-width:4px,color:#fff
            style 5 fill:#00FF00,stroke:#000,stroke-width:4px,color:#000
            ```   
    === "étape 1"
        ???+question "étape 1"
            ```mermaid              
            flowchart TB 
            1{R1} ---|1Gbits/s| 2(R2)
            2 --- |1Gbits/s|4(R4)
            1 ===> |1Mbits/s|3(R3) ---|1Gbits/s| 5
            4 ---|1Gbits/s| 5[/R5\]
            2 ---|1Gbits/s| 3
            
            subgraph Réseau
            1
            2
            3
            4
            5
            end

            style 1 fill:#BF0000,stroke:#000,stroke-width:4px,color:#fff
            style 5 fill:#00FF00,stroke:#000,stroke-width:4px,color:#000
            style 3 fill:#0000FF,stroke:#000,stroke-width:4px,color:#fff
            ```  
    === "étape 2"
        ???+done "étape 2"
            ```mermaid              
            flowchart TB 
            1{R1} ---|1Gbits/s| 2(R2)
            2 --- |1Gbits/s|4(R4)
            1 --- |1Mbits/s|3(R3) ===>|1Gbits/s| 5
            4 ---|1Gbits/s| 5[/R5\]
            2 ---|1Gbits/s| 3
            
            subgraph Réseau
            1
            2
            3
            4
            5
            end

            style 1 fill:#BF0000,stroke:#000,stroke-width:4px,color:#fff
            style 5 fill:#0000FF,stroke:#000,stroke-width:4px,color:#fff
            ```  
            > Le paquet n'a pas emprunté `R1 > R2 > R4 > R5` ou `R1 > R2 > R3 > R5` car il y a plus de sauts, même si le débit est très faible
    === "Erreur"
        ???+fail "Erreur"
            ```mermaid              
            flowchart TB 
            1{R1} ===>|1Gbits/s| 2(R2)
            2 --- |1Gbits/s|4(R4)
            1 --- |1Mbits/s|3(R3) ---|1Gbits/s| 5
            4 ---|1Gbits/s| 5[/R5\]
            2 ---|1Gbits/s| 3
            
            subgraph Réseau
            1
            2
            3
            4
            5
            end

            style 1 fill:#BF0000,stroke:#000,stroke-width:4px,color:#fff
            style 2 fill:#0000FF,stroke:#000,stroke-width:4px,color:#fff
            style 5 fill:#00FF00,stroke:#000,stroke-width:4px,color:#000
            ```  
            > Le paquet emprunte la route avec le moins de sauts, même si le débit est plus faible

### Cas 3
???+example "Cas 2"
    ???+info "Situation"
        Nous voulons transmettre le paquet du routeur **R1** au routeur **R17**
    
    === "Cas de base"
        ```mermaid              
        flowchart LR
        R1 --- R2
        R2 --- R3
        R3 --- R4
        R4 --- R5
        R5 --- R6
        R6 --- R7
        R7 --- R8
        R8 --- R9
        R9 --- R10
        R10 --- R11
        R11 --- R12
        R12 --- R13
        R13 --- R14
        R14 --- R15
        R15 --- R16
        R16 --- R17
        style R1 fill:#BF0000,stroke:#000,stroke-width:4px,color:#fff
        style R17 fill:#00FF00,stroke:#000,stroke-width:4px,color:#000
        ```  
    === "Solution"       
        ```mermaid              
        flowchart LR
        R1 --- R2
        R2 --- R3
        R3 --- R4
        R4 --- R5
        R5 --- R6
        R6 --- R7
        R7 --- R8
        R8 --- R9
        R9 --- R10
        R10 --- R11
        R11 --- R12
        R12 --- R13
        R13 --- R14
        R14 --- R15
        R15 --- R16
        R16 --- R17
        style R1 fill:#BF0000,stroke:#000,stroke-width:4px,color:#fff
        style R17 fill:#00FF00,stroke:#000,stroke-width:4px,color:#000
        ``` 
        ???+bug "Pas de solution"
            Il n'y a pas de solution, le protocole est fait pour que le **maximum** de sauts soit de **15**, or il y en a ici **16**.