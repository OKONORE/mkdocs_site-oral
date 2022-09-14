# 🎇 Conclusion

## Comparaison

???+note "Quelles sont les différences"
    |RIP|OSPF|
    |:-:|:-:|
    |Sauts limités à 15|Sauts illimités|
    |Organisation Simple|Organisation Complexe|
    |Très lourd a supporter pour les grands réseaux|Supporte les gros réseaux|
    |+ Efficace sur les petits réseaux|Machine à gaz sur les petits réseaux|
    |La charge ne peut pas être divisée|La charge peut se diviser|
    |Utilise les routes les plus courtes|Utilise les routes les plus rapides|

???+tldr "Contextes d'utilisations"
    |Type d'utilisation|Protocole RIP|Protocole OSPF|Justification|
    |:-:|:-:|:-:|:-:|
    |**dans une maison**|![image](/images/oui.jpg){ width=100 }|![image](/images/non.jpg){ width=100 }|Une maison n'a pas besoin que des aires soient définies, ce serait trop en faire|
    |**Dans une petite entreprise**|![image](/images/oui.jpg){ width=100 }|![image](/images/non.jpg){ width=100 }|Une petite entreprise ne dispose pas assez d'appareil pour avoir besoin que des aires soient définies|
    |**dans une grande entreprise**|![image](/images/non.jpg){ width=100 }|![image](/images/oui.jpg){ width=100 }|Une grande entreprise, avec plusieurs bâtiments, a besoin que des aires soient créé, afin d'optimiser les transferts entre ces bâtiments|
    |**Pour internet**|![image](/images/non.jpg){ width=100 }|![image](/images/oui.jpg){ width=100 }|Internet est tellement grand, que le protocol OSPF est obligatoire, sinon nous nous retrouverions avec des terraoctets de routes RIP transferées touutes les 30 secondes entre chaques routeur|
