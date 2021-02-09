# Technical Interview - Great Deals


## Introduction

The goal of the excercice is to set up a basic copy of the great deals website (https://greatdeals.myshopi.com/deals).  
The requirements all have priority (1 to 5, 1 being the most important and 5 being trivial). It is up to you to decide what you implement.  
There will a couple of restraints (database technology, asp.net core, graphql) but for the rest you have complete freedom in your choices.

### Development 

Development will be done on your own. If you have any questions contact us via slack

### Review

At the end of the excercice we will discuss what has been developed and what hasn't been developed.  
We suggest to go over the non-implemted features 30 minutes before the end of the excercice prepare a possible solution.  


### Requirements

- (1) We need to have an asp.net core website. (If you want to separate the app in frontend and backend you are free to do so.)  
- (1) There should be a homepage that shows all current great deals from the graphql endpoint (only the great deals for zoneid one should be shown)  
- (1) When I click on a great deal I should go to a details page
- (2)  When I go to the details page of a great deal a log event should be sent to an index in elasticsearch (credentials for elasticsearch are given, you can choose the datamodel for this. We should at least know the ip of the user, the page (url) and the date)
- (2) When I go to the details page of a great deal a log event should be sent to the database (credentials to a database are given, you can choose the datamodel for this.. We should at least know the ip of the user, the page (url) and the date)
- (2) There should be a dashboard in kibana to show the amount of events per day.
- (3) The website should run via a docker image
- (4) I should be able to switch languages (zoneId) in the website.
- (5) Whenever I check in on the main branch the solution should build


## Technical information

### Elasticsearch
username: elastic  
password: byAR0NCMeTmgl3JCUK3WtcQz  

elasticsearch: https://6604554c0fb04bf99a18a9cfacb3fd86.westeurope.azure.elastic-cloud.com:9243/  
kibana: https://60a65836446142ea97ab024ecfd2e341.westeurope.azure.elastic-cloud.com:9243/app/home#/  

### Sql database:
Database: bd-myshopi-interview.database.windows.net  
Admin user: interview  
Password: DWEF*holl9souy_waih   

SQL_Latin1_General_CP1_CI_AS

### application insights
instrumentation key: d9ad25fa-bdc8-40c7-9410-18c2216e1386  
Connection string: InstrumentationKey=d9ad25fa-bdc8-40c7-9410-18c2216e1386;IngestionEndpoint=https://westeurope-2.in.applicationinsights.azure.com/

### Data model

#### Great deals
agreat deal object has the following properties
- apiVersion: The version of the api the great deal supports (you can ignore this field)
- id: The identifier of the great deal 
- slug: The unique slug of the great deal
- image: The image to show on the details page of the great deal
- thumbnailTitle: the title to show in a list of great deals
- thumbnailDescription: the description to show in a list of great deals;
- title: the title to show in a details page of great deal
- description: The description to show in the details page of a great deal (Note that this is markdown)
- thumbnailImage : the picture to show in a list of great deals;
- price: The price the customer has to pay for a great deal
- originalPrice: the original price of this deal
- deliveryPrice: the price to deliver this item
- language: the language of this deal
- category: the category of the deal
- status: The status of this item (ONLINE or SOLDOUT)
- forAdults: A checkbox to indicate whether this box contains items only adults can buy (i.e. alcohol) (to ignore)
- bannerText: A text to show in the list to highlight a great deal (see the first great deal on the website (lotus))
- bannerColor: The background color of the banner

#### ZoneId: 
this field is used in the graphql query. A zoneId can be compared to a language where 
1= french (or fr) and 2= duch (or nl)

### GraphQL endpoint

Data will be provided by a graphql endpoint on the following url: http://api.myshopi.com/graphql  
IF you can't connect to graphql you can find the a json representation of the data under the query.


the query we need for great deals with zone Id 1 is the following:
```
query{
  greatDeals(zoneId: 1){
    apiVersion
    id
    slug
    image
    thumbnailTitle
    thumbnailDescription
    title
    description
    thumbnailImage
    price
    originalPrice
    deliveryPrice
    language
    category
    status
    forAdults
    bannerText
    bannerColor
  }
}

```


example data:



```json
{
  "data": {
    "greatDeals": [
      {
        "apiVersion": 1,
        "id": "45239640-780f-4570-b944-fa45fc38bd5e",
        "slug": "box-lotus-biscuits",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/myShopiGreatDeal-exploringbox-image-700x340px.png",
        "thumbnailTitle": "Boîte à biscuits Lotus  ",
        "thumbnailDescription": "Découvrez la meilleure gamme de biscuits Lotus et spéculoos !  ",
        "title": "Votre \"Boîte de biscuits Lotus\" pour 29,90 € au lieu de 41,10 €.",
        "description": "La combinaison idéale de biscuits Speculoos et d'autres produits délicieux Lotus. Idéal pour le café ou comme en-cas. Dégustez les Galettes Lotus Fines/Bretonnes et les spéculoos Lotus ! Vous recevrez également une tasse de café Lotus gratuitement et tous les biscuits sont livrés dans une boîte de rangement pratique. Achetez dès maintenant votre boîte de biscuits exclusive sur myShopi au prix réduit de 29,90 €.\r\n\r\n{#alert .alert .alert-warning}\r\nVous recevrez votre boîte au maximum 7 jours ouvrés après votre commande. (frais d'expédition = 2 €)\r\n\r\n#### Cette Boîte à biscuits Lotus contient :\r\n- Lotus Speculoos (100 pièces)\r\n- Lotus Galette Fine (50 pièces\r\n- Lotus Galette Bretonne (60 pièces)\r\n- Lotus Speculoos (150 pièces)\r\n- 1 tasse à café Lotus gratuite\r\n\r\nLes allergènes : Lotus Speculoos : contient du blé et du soja. Galettes fines : contient du blé et du soja.  Galette bretonne au beurre : contient du beurre, du blé, de l'œuf et du lait. Date d'expiration de la boîte : 26/04\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/Packshots.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/Packshots-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/Packshots-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/Packshots-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/Packshots-5.png)\r\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/myShopiGreatDeal-exploringbox-box-600x600px-sfeerfoto.png",
        "price": 29.9,
        "originalPrice": 41.1,
        "deliveryPrice": 2,
        "language": "fr",
        "category": "non-food",
        "status": "ONLINE",
        "forAdults": false,
        "bannerText": "NEW",
        "bannerColor": "#9FC6FF"
      },
      {
        "apiVersion": 1,
        "id": "b1e24544-778e-46af-809f-8e5cf9948ee4",
        "slug": "box-nalu-energy-2",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Green_header.6.png",
        "thumbnailTitle": "La boîte énergisante de Nalu  ",
        "thumbnailDescription": "Votre plein d'énergie dans une boîte avantageuse !   ",
        "title": "Votre \"boîte énergisante Nalu\" pour 32,67 € au lieu de 43,44 €  ",
        "description": "Nalu est l’allié parfaitement dosé contre le petit coup de mou de l’après-midi, après une longue journée de travail ou pendant des périodes d’étude ! Nalu Original est une boisson énergisante pétillante à base d'orange, pomme, kiwi et mangue.  Nalu black tea passionfruit, est une boisson énergisante à base d’extraits de thé noir et blanc et de jus de fruits (fruit de la passion et poire). Nalu green tea ginger, est une boisson énergisante à base d’extraits de thé vert, de jus de fruits (pomme et raisin blanc) avec une touche de gingembre. Les trois sont faibles en calories et enrichis en vitamines B3, E et B12 !\r\n\r\n{#alert .alert .alert-warning}\r\nVous recevrez votre boîte au maximum 7 jours ouvrés après votre commande. (frais d'expédition = 2 €)\r\n\r\n#### Cette boîte énergisante de Nalu en Edition Limitée contient :\r\n- 6 packs de Nalu Original 6x0.25L\r\n- GRATUIT : 1x0.25L Thé vert Nalu Gingembre + 1x0.25L Thé noir Nalu Fruit de la passion\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_Black_tea.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_Green_tea.png)\r\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/myShopi-box-rectangle_Nalu6pack.png",
        "price": 32.67,
        "originalPrice": 43.44,
        "deliveryPrice": 2,
        "language": "fr",
        "category": "non-food",
        "status": "ONLINE",
        "forAdults": false,
        "bannerText": null,
        "bannerColor": null
      },
      {
        "apiVersion": 1,
        "id": "7109fd0d-9ec3-4084-8132-38be5b05a328",
        "slug": "box-coca-cola-drinks",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola_drinks_Green_Header.png",
        "thumbnailTitle": "La boîte rafraichissante de Coca-Cola  ",
        "thumbnailDescription": "Votre boîte remplie de mini canettes pour un Maxi plaisir !  ",
        "title": "Votre boîte rafraichissante pour 37,74 € au lieu de 50,32€.  ",
        "description": "Profitez d'un vaste choix de boissons pour un délicieux moment rafraichissant !  Grâce à cette boîte remplie de mini canettes, vous profiterez du goût original, authentique et emblématique de Coca-Cola. Fanta, également présent dans la boîte, désaltèrera toute la famille grâce à sa recette rafraichissante, pétillante, et fabriquée à base d'arômes naturels. Le goût du fun à l'état pur !  Enfin, Sprite, avec son goût relevé, pétillant, et ses saveurs naturelles de citron et citron vert, vous offrira une expérience aussi intense que rafraichissante.  \r\n\r\n{#alert .alert .alert-warning}\r\nVous recevrez votre boîte au maximum 7 jours ouvrés après votre commande. (frais d'expédition = 2 €)\r\n\r\n#### Cette boîte rafraichissante contient :\r\n- 4 packs Coca-Cola Regular minicans 0.15L (48 mini canettes)\r\n- 2 packs Sprite minican 0.15L (24 mini canettes)\r\n- 2 packs Fanta Orange minican 0.15L (24 mini canettes)\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Sprite.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Sprite.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Fanta.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Fanta.png)",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola_drinks_box-2.png",
        "price": 37.74,
        "originalPrice": 50.32,
        "deliveryPrice": 2,
        "language": "fr",
        "category": "non-food",
        "status": "ONLINE",
        "forAdults": false,
        "bannerText": null,
        "bannerColor": null
      },
      {
        "apiVersion": 1,
        "id": "ddde95a7-dc5f-4a23-82ae-000000000002",
        "slug": "box-summer-refreshing",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-image-700x340px.png",
        "thumbnailTitle": "La \"Boîte Rafraichissante\"\r\n",
        "thumbnailDescription": "Tout ce dont vous avez besoin pour une journée rafraîchissante !",
        "title": "Votre \"Boîte Rafraichissante\" à 24,95 € au lieu de 33,45 €\r\n",
        "description": "#### Besoin d'un rafraîchissement \r\nAlors vous devriez absolument commander cette \"\"Boîte Rafraichissante\"\" ! Cette boîte est remplie de délicieuses boissons hydratantes que vous devriez essayer. L'eau de B-Better, les sachets  aux herbes pour eau froide et l'ice tea à la pêche de Lipton tout en gardant votre niveau d'énergie élevé grâce à la boisson énergétique Yula Amazonian. Vous pouvez désormais acheter cette boîte rafraîchissante pour 24,95 € au lieu de 33,45 €. \r\n\r\n{#alert .alert .alert-warning}\r\nVous recevrez votre boîte au maximum 7 jours ouvrables après votre commande. (frais d'expédition = 2 €)\r\n\r\n#### Cette \"Boîte Rafraichissante Estivale\" contient : \r\n- Pyramides de Lipton Herbes d'eau froide Hibiscus Grenade 15 sachets\r\n- Pyramides de Lipton Herbes pour eau froide Citron Camomille 15 sachets\r\n- B-Better Water Charcoal 750ml\r\n- B-Better Water Beauty 750ml\r\n- B-Meilleure énergie de l'eau 750ml\r\n- Lipton Ice Tea pétillant Ice Tea ORIGINAL 6x 50 cl\r\n- Thé glacé Lipton ice tea non pétillant Pêche 6x 50 cl\r\n- Yula Amazonian Energy Drink Mango & Chili Pepper 25cl\r\n\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-8.png)\r\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-box-600x600px.png",
        "price": 24.95,
        "originalPrice": 33.45,
        "deliveryPrice": 2,
        "language": "fr",
        "category": "non-food",
        "status": "ONLINE",
        "forAdults": false,
        "bannerText": null,
        "bannerColor": null
      },
      {
        "apiVersion": 1,
        "id": "26ea1242-dad8-4b58-b2de-7e6062297afd",
        "slug": "box-bebe-zwitsal",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-image-700x340px.png",
        "thumbnailTitle": "Boîte Zwitsal Baby",
        "thumbnailDescription": "Tout ce dont vous avez besoin pour la peau sensible de votre bébé",
        "title": "Votre boîte Zwitsal Baby pour seulement 43,99€ au lieu de 66,95€",
        "description": "\n{#alert .alert .alert-warning}\nLes frais de livraison sont GRATUIT pour les 300 premières commandes\n\nRien n'est plus doux que la peau de votre bébé. Et tout comme vous, nous voulons garder cette douceur le plus longtemps possible. C'est pourquoi Zwitsal veille depuis près de 100 ans à ce que ses produits de soins pour bébés soient adaptés aux propriétés particulières de la peau des bébés. Avec la boîte Zwitsal Baby, vous découvrirez un certain nombre de produits haut de gamme de la gamme Zwitsal à un prix abordable.\n\nCette boîte Zwitsal Baby contient :\n\n- Zwitsal Lotion Corporelle 400 ml\n- Zwitsal Parfum Eau de Zwitsal 95 ml\n- Zwitsal Shampooing 400 ml\n- Zwitsal Gel lavant Sans Savon 400 ml\n- Zwitsal Kids Lingettes Humides Disney 40 pièces\n- Zwitsal Lingettes Humides Lotion 12 x 65 pièces\n\n\n\n\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-1.png)\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-2.png)\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-3.png)\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-4.png)\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-5.png)\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-7.png)\n\nVous recevrez votre coffret au maximum 7 jours ouvrés après votre commande.\n\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-box-600x600px-sfeerfoto.png",
        "price": 43.99,
        "originalPrice": 66.95,
        "deliveryPrice": 4.9,
        "language": "fr",
        "category": "non-food",
        "status": "ONLINE",
        "forAdults": false,
        "bannerText": null,
        "bannerColor": null
      },
      {
        "apiVersion": 1,
        "id": "f13e95a7-dc5f-4a23-0002-000000000001",
        "slug": "box-exploring",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/myShopiGreatDeal-exploringbox-image-700x340px.png",
        "thumbnailTitle": "La Boîte Exploration ",
        "thumbnailDescription": "Découvrez tous les produits à plus de 60% de réduction ! ",
        "title": "Votre \"Boîte Exploration\" pour 14,95 € !",
        "description": "Découvrez plein de produits grâce à la Boîte Exploration de myShopi ! Une boîte remplie de SUPERS marques que vous pouvez acheter avec plus de 60% de réduction ! Pour 14,95 € livraison incluse, vous ne recevrez pas moins de 18 produits, un échantillon gratuit et un bon de réduction. Si ce n'est pas un \"\"great deal\"\" !\r\n\r\n{#alert .alert .alert-warning}\r\nVous recevrez votre boîte au maximum 7 jours ouvrés après votre commande. (Coût de livraison inclus)\r\n\r\n#### Cette Boîte Exploration contient :  \r\n1x Cornet 33cl, 1x Rodenbach fruitage 33cl, 1x Arizona sparkling 330ml, 1x Vitamin well reload 500ml, 1x Moonpop sweet & salty 35gr, 1x Neutrogena hydro boost masque hydrogel 30gr, 1x Devos lemmens sauce bbq burger 240ml, 8x canettes Coca-Cola light 25cl, 1x canette Coca-Cola energy 25cl, 1x canette Monster Espresso milk 25cl, 1x Mc Vitie's Original 250gr, 1x Blistex medplus stick 4,25gr, 1x Jordan dental sticks extra fin 17gr, 1x Hero baby pomme-fraise 90gr, 1x Miracoli tomato & basilic sauce 170gr, 1x Granini ace 1l + coupon, 1x Lay's four veggie beetroot 100gr, 1x Alpro cooking avoine 250gr  \r\n*+  Echantillon gratuit de Lenor Unstoppables 23gr\r\n\r\n\r\nAllergènes:  \r\nCornet: Malt d'orge, blé  \r\nRodenbach fruitage: Malt d'orge  \r\nDevos lemmens sauce bbq burger: Moutarde, oeuf  \r\nMc Vitie's Original: Gluten, lait  \r\nAlpro cooking avoine: avoine  \r\n\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-8.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-9.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-10.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-11.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-12.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-13.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-14.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-15.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-16.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-17.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-18.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-19.png)",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/myShopiGreatDeal-exploringbox-box-600x600px-sfeerfoto.png",
        "price": 14.95,
        "originalPrice": 38.06,
        "deliveryPrice": 0,
        "language": "fr",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      },
      {
        "apiVersion": 1,
        "id": "7583a817-72a9-491c-af93-708e893f86c3",
        "slug": "box-saint-nicolas",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/myShopiGreatDeal-Sinterklaaspx.png",
        "thumbnailTitle": "La boîte 100% Saint-Nicolas  ",
        "thumbnailDescription": "La combinaison idéale de spéculoos, de nic-nacs et de chocolat pour la Saint-Nicolas.  ",
        "title": "Votre boîte Saint-Nicolas pour 37,90 € au lieu de 47,50 €  ",
        "description": "Dans cette composition, l'amateur de biscuits et de chocolat peut trouver tout ce qu'il souhaite pour la Saint-Nicolas. Les figurines de Libeert sont emballées dans un seau réutilisable de 6L avec un couvercle en plastique. Les lettres nic-nac des Folies De Lowie sont aussi emballées dans un pot en plastique avec un couvercle pratique. Les figurines et les nic-nacs sont accompagnés de deux incontournables de Lotus, à savoir les mini Saint-Nicolas avec 6 sachets de 25 grammes et une boîte pliante avec 5 références de spéculoos supérieurs. Les deux peuvent ainsi servir de boîtes de stockage permanentes. De cette façon, l'emballage a une seconde vie durable après la consommation. \r\n\r\n{#alert .alert .alert-warning}\r\nVous recevrez votre boîte au maximum 7 jours ouvrés après votre commande. (frais d'expédition = 2 €)\r\n\r\n#### Cette boîte Saint-Nicolas contient :\r\n- 20 pièces de figurines de Saint-Nicolas en chocolat Libeert* (700g)\r\n- Les Folies de Lowie Nic-Nacs* (400g)\r\n- Les mini Saint Nicolas Lotus (150g)\r\n- Boite de Saint-Nicolas Lotus (180g)\r\n\r\n*Avec des emballages réutilisables et durables qui ont une seconde vie après la consommation !\r\n\r\nNic-Nacs : Peut contenir de l'œuf, du soja et du sésame. Figures creuses en chocolat : Contient du lait et du soja. Peut contenir des traces d'œufs, de gluten et de noix. Boite de Saint-Nicolas Lotus : contient du gluten, du soja et du lait.  Les mini Saint-Nicolas Lotus : Contient du lait et du soja.\r\n\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/Packshots.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/Packshot.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/Packshots-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/Packshots-3.png)",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/myShopiGreatDeal-hygienebox-box-600x600px.png.png",
        "price": 37.9,
        "originalPrice": 47.5,
        "deliveryPrice": 2,
        "language": "fr",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      },
      {
        "apiVersion": 1,
        "id": "ddde95a7-dc5f-4a23-82ae-000000000012",
        "slug": "box-summer-beauty",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-image-700x340px.png",
        "thumbnailTitle": "La \"Boîte Beauté\"\r\n",
        "thumbnailDescription": "Démarrez la journée en beauté !",
        "title": "Votre \"Boîte Beauté\" à 26,90 € au lieu de 41,72 €\r\n",
        "description": "Cette \"\"Boîte Beauté\"\" vous aidera à profiter des mois d'été en toute fraîcheur. Des dents saines avec le dentifrice et le bain de bouche Signal, des bons soins pour la peau grâce au gel douche , au déodorant et à la lotion corporelle Dove, aux cheveux doux et revitalisés avec le shampooing et l'après-shampooing Timotei. Commandez dès maintenant tous ces produits phares pour seulement 26,90 € au lieu de 41,72 € !\r\n\r\n{#alert .alert .alert-warning}\r\nVous recevrez votre boîte au maximum 7 jours ouvrables après votre commande. (frais d'expédition = 2 €)\r\n\r\n#### Cette \"Boîte Beauté\" contient : \r\n- Dove déodorant en spray Invisible Dry 150 ml\r\n- Dove Limited edition Douchegel Summer Breeze 250 ml\r\n- Lait corporel Dove Soin nourrissant pour le corps SPF 15 250 ml\r\n- Dove Derma Spa Body Mousse Summer Revived Fair 150 ml\r\n- Signal Integral 8 Tandpasta Charbon de bois avec l'aide de l'équipe\r\n- Signal Mondwater Pro Complete 500 ml\r\n- Timotei Conditioner Pure Nourished & Light 300 ml\r\n- Shampooing Timotei Pure Nourrie & légère à la noix de coco 300 ml\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-8.png)\r\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-box-600x600px.png",
        "price": 26.9,
        "originalPrice": 41.72,
        "deliveryPrice": 2,
        "language": "fr",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      },
      {
        "apiVersion": 1,
        "id": "d13e95a7-dc5f-4a23-82ae-f0613f360002",
        "slug": "box-d-drinks-decouverte",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-image-700x340px.png",
        "thumbnailTitle": "La \"Boîte Cuisine Découverte D-Drinks\"\r\n",
        "thumbnailDescription": "Prêt à partir en voyage découverte ? \r\n",
        "title": "Votre \"Boîte Cuisine Découverte D-Drinks\" à 23,50 € au lieu de 35 €\r\n",
        "description": "Êtes-vous prêt à partir pour un grand voyage découverte ? La Boîte Cuisine Découverte D-Drinks est remplie de boissons et de snacks grâce auxquels vous pourrez vivre des moments \"\"oh, c'est plus savoureux et plus sain que ce à quoi je suis habitué !\"\".\r\nLa boîte contient les dernières nouveautés en matière de boissons plus saines et naturelles, sans pour autant sacrifier le goût.\r\nPartagez un délicieux sac de pop-corn lors d'un apéro ou dégustez une limonade fraîche après une chaude journée d'été, vous trouverez tout cela dans une grande boîte élégante. Il y en a pour tous les goûts !\r\nA votre santé !\r\n\r\n{#alert .alert .alert-warning}\r\nVous recevrez votre boîte au maximum 7 jours ouvrables après votre commande. (frais d'expédition = 1 €)\r\n\r\n\r\n#### Cette \"Boîte Cuisine Découverte D-Drinks\" contient : \r\n- Green Tea Ginseng & Honey 330ML CAN\r\n- Green Tea Pomegranate 330ML PET\r\n- Juice Shot Ginger & Lemon 60ML GLASS\r\n- Reload Lemon & Lime 500ML PET\r\n- Sparkling Ginger 330ML CAN\r\n- KIDS APPLE PEAR 200ML TETRA\r\n- KIDS SUMMER BERRIES 200ML TETRA\r\n- Raspberry 250ML CAN\r\n- MINI 150ML CAN\r\n- Natural Energy 250ML CAN\r\n- Apple Kiwi 500ml PET\r\n- Hazelnut & Nougat 55g BAR\r\n- Pop Corn Crisps Sea Salt 28G SINGLE SERVE\r\n- Lovely Sweet 'n Salty Popcorn 35G SINGLE SERVE\r\n- Vanilla & Choc Chip 55G BAR\r\n- Hazelnut Crunch 33G SNACKING PACK\r\n\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-8.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-9.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-10.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-11.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-12.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-13.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-14.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-15.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-16.png)\r\n\r\n\r\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-box-600x600px-v2.png",
        "price": 23.5,
        "originalPrice": 35,
        "deliveryPrice": 1,
        "language": "fr",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      },
      {
        "apiVersion": 1,
        "id": "d13e95a7-dc5f-4a23-82ae-f0613f350001",
        "slug": "box-d-drinks-famille",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-image-700x340px.png",
        "thumbnailTitle": "La \"Boîte Cuisine Famille D-Drinks\"",
        "thumbnailDescription": "Découvrez cette boîte avec toute la famille !",
        "title": "Votre \"Boîte Cuisine Famille D-Drinks\" à 43,90 € au lieu de 50 €",
        "description": "Êtes-vous prêt à partir pour un grand voyage découverte avec toute la famille ? La Boîte Cuisine Famille D-Drinks est remplie de boissons et de snacks grâce auxquels vous vivrez des moments \"\"oh, c'est plus savoureux et plus sain que ce à quoi je suis habitué !\"\" - des moments à vivre en famille.\r\nLa boîte contient les dernières nouveautés en matière de boissons plus saines et naturelles, sans avoir à sacrifier le goût.\r\nPartagez un délicieux sac de pop-corn lors d'un apéro ou dégustez une limonade fraîche après une chaude journée d'été, vous trouverez tout cela dans une grande boîte élégante. Il y en a pour tous les goûts !\r\nA votre santé !\r\n\r\n{#alert .alert .alert-warning}\r\nVous recevrez votre boîte au maximum 7 jours ouvrables après votre commande. (frais d'expédition = 1 €)\r\n\r\n\r\n#### Cette \"Boîte Cuisine Famille D-Drinks\" contient : \r\n- Green Tea Ginseng & Honey 500ml PET\r\n- Kiwi strawberry 500ml PET\r\n- Raspberries 330ML CAN \r\n- Green Tea Pomegranate 330ML PET\r\n- Juice Shot Turmeric, Ginger & Pineapple 60ML GLASS\r\n- Juice Shot Ginger & Lemon 60ML GLASS\r\n- Reload Lemon & Lime 500ML PET\r\n- Defence Citrus & Elderflower 500ML PET\r\n- 500ml\r\n- Sparkling Ginger 330ML CAN\r\n- Sparkling Apple &Rhubarb 330ML CAN\r\n- KIDS APPLE PEAR 200ML TETRA\r\n- KIDS SUMMER BERRIES 200ML TETRA\r\n- Raspberry 250ML CAN\r\n- Kiwi, Lime & Mint 400ML PET\r\n- MINI 150ML CAN\r\n- Alcohol Free Rhubarb G&T 250ML CAN\r\n- Iced Berry 500ml PET\r\n- Hazelnut & Nougat 55g BAR\r\n- Pop Protein Crisps Sweet Barbecue 28G SINGLE SERVE\r\n- Lovely Sweet 'n Salty Popcorn 35G SINGLE SERVE\r\n- Vanilla & Choc Chip 55G BAR\r\n- Blueberries & Chia Seeds Fruit 45G BAR\r\n- Freeze-Dried Peach 20G BAG\r\n- Sour Flower 100G SHARING BAG\r\n- Hazelnut Crunch 33G SNACKING PACK\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-8.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-9.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-10.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-11.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-12.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-13.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-14.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-15.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-16.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-17.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-18.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-19.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-20.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-21.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-22.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-23.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-24.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-25.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-26.png)\r\n\r\n\r\n\r\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-box-600x600px-v2.png",
        "price": 43.9,
        "originalPrice": 50,
        "deliveryPrice": 1,
        "language": "fr",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      },
      {
        "apiVersion": 1,
        "id": "00000078-dc5f-4a23-82ae-f0613f3c9ba7",
        "slug": "box-food-of-the-world",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-image-700x340px.png",
        "thumbnailTitle": "Boîte Cuisine \"Food of the World\"\r\n",
        "thumbnailDescription": "Laissez-vous inspirer par la cuisine du monde !\r\n",
        "title": "Votre Boîte de Cuisine \"Food of the World\" pour 29,99 € au lieu de 51 €.\r\n",
        "description": "Pas d'inspiration pour cuisiner ? Et vous aimeriez apporter un sentiment de vacances dans votre cuisine ? \r\nQue pensez-vous des tacos mexicains, du wok asiatique, du tikka masala indien ou des  poké bols hawaïens ? \r\nAvec cette boîte de cuisine \"Food of the World\", vous pouvez concocter les plats les plus délicieux et faire entrer le monde dans votre cuisine.\r\nEn plus des ingrédients de ces plats, vous trouverez également 4 fiches de recettes dans votre boîte de cuisine \"Food of the World\". \r\nCelles-ci expliquent à chaque étape simple comment préparer ce repas simple en seulement 25 minutes. Bon appétit !\r\n\r\n{#alert .alert .alert-warning}\r\nVous recevrez votre boîte au maximum 7 jours ouvrable après votre commande. (frais d'expédition = 2 €)\r\n\r\n#### Cette Boîte Cuisine \"Box Saveur Du Monde\" contient :\r\n- Suzi Wan lait de coco (500 ml)\r\n- Suzi Wan purée de piments (100 gr)\r\n- Suzi Wan nouilles en nids (250 gr)\r\n- Sharwoods Sauce Tikka Masala (420 gr)\r\n- Sharwoods Poppadoms (paquet de 8)\r\n- Sharwoods Green Label Mango Chutney (227 gr)\r\n- Blue Dragon riz de sushi (500 gr)\r\n- Blue Dragon vinaigre de riz (150 ml)\r\n- Blue Dragon Japanse sauce soja (150 ml)\r\n- Blue Dragon gingembre sushi (145 gr)\r\n- Old El Paso taco shells (paquet de 12)\r\n- Old El Paso sliced Jalapeños verts(215 gr)\r\n- Old El Paso mélange aux épices pour tacos (25 gr)\r\n- Old El Paso sauce taco (235 gr)\r\n\r\n\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-8.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-9.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-10.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-11.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-12.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-13.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-14.png)",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-box-600x600px-FR.png",
        "price": 29.99,
        "originalPrice": 51,
        "deliveryPrice": 2,
        "language": "fr",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      },
      {
        "apiVersion": 1,
        "id": "45239640-780f-4570-b944-fa45fc38bd5f",
        "slug": "lotus-koekjesbox",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/myShopiGreatDeal-exploringbox-image-700x340px.png",
        "thumbnailTitle": "Lotus Koekjesbox  ",
        "thumbnailDescription": "Ontdek het beste Lotus koek- en Speculoosgamma!  ",
        "title": "Jouw \"Lotus Koekjesbox\" voor €29,90 i.p.v. €41,10  ",
        "description": "De ideale combinatie van Speculooskoekjes en andere lekkernijen van Lotus. Ideaal voor bij de koffie of als tussendoortje. Geniet van de Lotus Galettes Fijn/Bretonnes en van Lotus gewone speculoos! Je ontvangt bovendien gratis een Lotus Koffietas en alle koekjes zitten in een handige opbergdoos. Koop jouw exclusieve koekjesdoos nu op myShopi aan een voordelige prijs van €29,90.\r\n\r\n{#alert .alert .alert-warning}\r\nJe ontvangt jouw box maximum 7 werkdagen na jouw bestelling. (verzendkosten = € 2)\r\n\r\n#### Deze Lotus Koekjesbox bevat:\r\n- Lotus Speculoos (100 stuks)\r\n- Lotus Galette Fijn (50 stuks)\r\n- Lotus Galette Bretonne (60 stuks) \r\n- Lotus Speculoos (150 stuks)\r\n- 1 gratis Lotus koffietas\r\n\r\nAllergenen: Lotus Speculoos: bevat tarwe en soja. Fijne galetten: bevat tarwe en soja.  Bretons botergaletje: bevat boter, tarwe, ei en melk. Houdsbaarheiddatum doos: 26/04\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/Packshots.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/Packshots-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/Packshots-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/Packshots-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/Packshots-5.png)\r\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/lotus-cookies-box/myShopiGreatDeal-exploringbox-box-600x600px-sfeerfoto.png",
        "price": 29.9,
        "originalPrice": 41.1,
        "deliveryPrice": 2,
        "language": "nl",
        "category": "non-food",
        "status": "ONLINE",
        "forAdults": false,
        "bannerText": "NEW",
        "bannerColor": "#9FC6FF"
      },
      {
        "apiVersion": 1,
        "id": "b1e24544-778e-46af-809f-8e5cf9948ee3",
        "slug": "nalu-energy-box-2",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Green_header.6.png",
        "thumbnailTitle": "Nalu Energy Box  ",
        "thumbnailDescription": "Jouw portie energie in een voordeeldoos!   ",
        "title": "Jouw \"Nalu Energy Box\" voor € 32,67 i.p.v. € 43,44  ",
        "description": "Nalu is het perfecte drankje tegen een middagdip, na een lange werkdag of tijdens de studieperiode! Nalu Original is een sprankelende energiedrank gemaakt van sinaasappel, appel, kiwi en mango. Nalu zwarte thee passievrucht, is een energiedrank op basis van zwarte en witte thee-extracten en vruchtensappen (passievrucht en peer). Nalu groene thee gember, is een energiedrank op basis van groene thee-extracten, vruchtensappen (appel en witte druif) met een vleugje gember. Alle drie zijn ze caloriearm en verrijkt met vitamine B3, E en B12!\r\n\r\n{#alert .alert .alert-warning}\r\nJe ontvangt jouw box maximum 7 werkdagen na jouw bestelling. (verzendkosten = € 2)\r\n\r\n#### Deze 'Nalu Energy Box Limited Edition' bevat:\r\n- 6 packs van Nalu Original 6x0.25L\r\n- GRATIS: 1x0.25L Nalu Groene thee Gember + 1x0.25L Nalu Zwarte Thee Passievrucht\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_x6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_Black_tea.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/Nalu_Green_tea.png)",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/nalu-box/myShopi-box-rectangle_Nalu6pack.png",
        "price": 32.67,
        "originalPrice": 43.44,
        "deliveryPrice": 2,
        "language": "nl",
        "category": "non-food",
        "status": "ONLINE",
        "forAdults": false,
        "bannerText": null,
        "bannerColor": null
      },
      {
        "apiVersion": 1,
        "id": "7109fd0d-9ec3-4084-8132-38be5b05a327",
        "slug": "coca-cola-drinks-box",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola_drinks_Green_Header.png",
        "thumbnailTitle": "Verfrissingsbox van Coca-Cola  ",
        "thumbnailDescription": "Jouw doos vol met mini-blikjes voor een Maxi-plezier!  ",
        "title": "Jouw \"verfrissingsbox\" voor €37,74 i.p.v. € 50,32  ",
        "description": "Geniet van een ruime keuze aan drankjes voor een heerlijk en verfrissend moment!  Dankzij deze doos gevuld met mini-blikjes geniet je van de originele, authentieke en emblematische smaak van Coca-Cola. Fanta, ook aanwezig in de doos, zal de dorst van het hele gezin lessen dankzij het verfrissende, sprankelende recept met natuurlijke smaken. Kortom, de smaak van puur plezier!  Tot slot zal Sprite, met zijn kruidige, sprankelende smaak van natuurlijke citroen en limoen, je een even intense als verfrissende ervaring bieden. \r\n\r\n{#alert .alert .alert-warning}\r\nJe ontvangt jouw box maximum 7 werkdagen na jouw bestelling. (verzendkosten = € 2)\r\n\r\n#### Deze verfrissingsbox bevat:\r\n- 4 packs Coca-Cola Regular minicans 0.15L (48 mini-blikjes)\r\n- 2 packs Sprite minican 0.15L (24 mini-blikjes)\r\n- 2 packs Fanta Orange minican 0.15L (24 mini-blikjes)\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Sprite.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Sprite.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Fanta.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Fanta.png)",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/coca-cola-drinkgs-box/Coca-cola_drinks_box-2.png",
        "price": 37.74,
        "originalPrice": 50.32,
        "deliveryPrice": 2,
        "language": "nl",
        "category": "non-food",
        "status": "ONLINE",
        "forAdults": false,
        "bannerText": null,
        "bannerColor": null
      },
      {
        "apiVersion": 1,
        "id": "ddde95a7-dc5f-4a23-82ae-000000000001",
        "slug": "summer-refreshing-box",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-image-700x340px.png",
        "thumbnailTitle": "De \"Refreshing Box\"\r\n",
        "thumbnailDescription": "Verfrissend de dag in! ",
        "title": "Jouw \"Refreshing Box\" voor € 24,95 i.p.v. € 33,45\r\n",
        "description": "#### Nood aan verfrissing? \r\nDan moet je deze \"Refreshing Box\" absoluut bestellen! Deze box zit boordevol lekkere hydraterende drankjes die je moet proberen. B-Better water, Lipton kruiden pyramides voor koud water, Lipton ice tea perzik en hou je energie op peil dankzij Yula Amazonian Energy Drink. Deze verfrissende box kan je nu aankopen voor €24,95 i.p.v €33,45. \r\n\r\n{#alert .alert .alert-warning}\r\nJe ontvangt jouw box maximum 7 werkdagen na jouw bestelling. (verzendkosten = € 2)\r\n\r\n#### Deze \"Refreshing Box\" bevat:\r\n- Lipton Pyramides Kruiden voor koud water Hibiscus Pomegranate 15 zakjes\r\n- Lipton Pyramides Kruiden voor koud water Lemon Camomile 15 zakjes\r\n- B-Better Water Charcoal 750ml\r\n- B-Better Water Beauty 750ml\r\n- B-Better Water Energy 750ml\r\n- Lipton Ice Tea Bruisend Ice Tea ORIGINAL 6x 50 cl\r\n- Lipton Ice Tea Niet Bruisend Ice Tea Perzik 6x 50 cl\r\n- Yula Amazonian Energy Drink Mango & Chili Pepper 25cl\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-packshotsAantal-100x100px-8.png)\r\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/summer-refreshing-box-unilever/myShopiGreatDeal-Unilever-RefreshingSummerBox-box-600x600px.png",
        "price": 24.95,
        "originalPrice": 33.45,
        "deliveryPrice": 2,
        "language": "nl",
        "category": "non-food",
        "status": "ONLINE",
        "forAdults": false,
        "bannerText": null,
        "bannerColor": null
      },
      {
        "apiVersion": 1,
        "id": "3e79c4ad-1bca-4701-a543-e0e1ffe4ce61",
        "slug": "zwitsal-baby-box",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-image-700x340px.png",
        "thumbnailTitle": "Zwitsal Baby Box",
        "thumbnailDescription": "Alles wat je nodig hebt voor de gevoelige huid van jouw baby",
        "title": "Jouw Zwitsal Baby Box voor slechts €43,99 i.p.v. €66,95",
        "description": "{#alert .alert .alert-warning}\nDe eerste 300 Zwitsal Baby Boxen worden gratis verzonden!\n\nNiets is zo zacht als de huid van jouw baby. En net als jij willen wij dat zo houden. Daarom zorgt Zwitsal er al bijna 100 jaar voor dat hun babyverzorgingsproducten passen bij de speciale eigenschappen van het babyhuidje. Met de Zwitsal Baby Box ontdek jij een aantal topproducten uit het Zwitsal gamma tegen een voordelige prijs.\n\nDeze Zwitsal Baby Box bevat:\n\n- Zwitsal Body Lotion 400 ml\n- Zwitsal Parfum Eau de Zwitsal 95 ml\n- Zwitsal Shampoo 400 ml\n- Zwitsal Wasgel Zeepvrij 400 ml\n- Zwitsal Kids Vochtige Doekjes Disney 40 stuks\n- 12x Zwitsal Vochtige Doekjes Lotion 65 stuks\n\n\n\n\n\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-1.png)\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-2.png)\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-3.png)\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-4.png)\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-5.png)\n![product](https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-packshotsAantal-100x100px-7.png)\n\n\nJe ontvangt jouw box maximum 7 werkdagen na jouw bestelling.\n\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/baby-box/myShopiGreatDeal-babybox-box-600x600px-sfeerfoto.png",
        "price": 43.99,
        "originalPrice": 66.95,
        "deliveryPrice": 4.99,
        "language": "nl",
        "category": "non-food",
        "status": "ONLINE",
        "forAdults": false,
        "bannerText": null,
        "bannerColor": null
      },
      {
        "apiVersion": 1,
        "id": "f13e95a7-dc5f-4a23-0001-000000000001",
        "slug": "exploring-box",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/myShopiGreatDeal-exploringbox-image-700x340px.png",
        "thumbnailTitle": "De Exploring box",
        "thumbnailDescription": "Ontdek alle producten met meer dan 60% korting! ",
        "title": "Jouw \"Exploring box\" voor € 14,95!",
        "description": "Ga op ontdekking dankzij de myShopi Exploring box! Een box boordevol TOP merken die je kan uitproberen met meer dan 60% korting! Voor € 14,95 inclusief verzending krijg je maar liefst 18 producten, een gratis sample en coupon. Als dat geen \"\"great deal\"\" is!\r\n\r\n{#alert .alert .alert-warning}\r\nJe ontvangt jouw box maximum 7 werkdagen na jouw bestelling. (inclusief verzendingskosten)\r\n\r\n\r\n#### Deze 'Exploring box' bevat: \r\n1x Cornet 33cl, 1x Rodenbach fruitage 33cl, 1x Arizona sparkling 330ml, 1x Vitamin well reload 500ml, 1x Moonpop sweet & salty 35gr, 1x Neutrogena hydro boost hydrogel masker 30gr, 1x Devos lemmens schenksaus bbq burger 240ml, 8x Coca-cola light blikjes 25cl, 1x Coca-cola energy blik 25cl, 1x Monster espresso milk blik 25cl, 1x Mc Vitie's Original 250gr, 1x Blistex medplus stick 4,25gr, 1x Jordan dental sticks extra fijn 17gr, 1x Hero baby appel aardbei 90gr, 1x Miracoli tomato & basilicum saus 170gr, 1x Granini ace 1l + coupon, 1x Lay's oven veggie beetroot 100gr, 1x Alpro cooking haver 250gr  \r\n*+ Gratis sample Lenor Unstoppables 23gr \r\n\r\n\r\nAllergenen:  \r\nCornet: Gerstemout, tarwe  \r\nRodenbach fruitage: Gerstemout  \r\nDevos lemmens schenksaus bbq burger: Mosterd, ei  \r\nMc Vitie's Original: Gluten, milk  \r\nAlpro cooking haver: haver  \r\n\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-8.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-9.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-10.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-11.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-12.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-13.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-14.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-15.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-16.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-17.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-18.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/Packshots-19.png)",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/exploring-box/myShopiGreatDeal-exploringbox-box-600x600px-sfeerfoto.png",
        "price": 14.95,
        "originalPrice": 38.06,
        "deliveryPrice": 0,
        "language": "nl",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      },
      {
        "apiVersion": 1,
        "id": "7583a817-72a9-491c-af93-708e893f86c2",
        "slug": "sinterklaas-box",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/myShopiGreatDeal-Sinterklaaspx.png",
        "thumbnailTitle": "All-in-one Sinterklaas Box  ",
        "thumbnailDescription": "De ideale combinatie van speculaas, nic-nac lettertjes en chocolade voor Sinterklaas   ",
        "title": "Jouw \"Sinterklaas Box\" voor € 37,90 i.p.v. € 47,50  ",
        "description": "In deze combinatie kan de koek- en chocoladeliefhebber alles terugvinden wat hij/zij kan verlangen voor Sinterklaas. De holle figuren van Libeert zijn verpakt in een herbruikbare emmer van 6l met dekselsluiting van kunststof. De nic-nac lettertjes van Les Folies De Lowie zijn verpakt in een bokaal van kunststof met handige dekselsluiting. De holle figuren en nic-nacs worden vergezeld met twee toppers van Lotus, zijnde de mini Klaasmannen met 6 zakjes van 25 gram en een klaasdoosje met 5 top speculaas referenties. Beiden kunnen dus dienen als permanente bewaardozen. Op deze manier krijgen de verpakkingen na consumptie een duurzaam tweede leven. \r\n\r\n{#alert .alert .alert-warning}\r\nJe ontvangt jouw box maximum 7 werkdagen na jouw bestelling. (verzendkosten = € 2)\r\n\r\n#### Deze Sinterklaas Box bevat:\r\n- 20 stuks Libeert Sinterklaas holle chocolade* (700g)\r\n- Les Folies de Lowie Nic-Nacs lettertjes* (400g)\r\n- Lotus mini Klaasmannen (150g)\r\n- Lotus Sinterklaasdoosje (180g)\r\n\r\n\r\n*Met herbruikbare en duurzame verpakkingen die na consumptie een tweede leven krijgen!\r\n\r\nNic-Nacs: Kan ei, soja en sesam bevatten. Chocolade holle figuren: Bevat melk en soja. Kan sporen bevatten van eieren, gluten en noten. Lotus Klaasdoosje: Bevat gluten, soja en melk.  Lotus mini Klaasmannen: bevat Melk en soja.\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/Packshots.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/Packshot.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/Packshots-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/Packshots-3.png)",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/sinterklaas-box/myShopiGreatDeal-hygienebox-box-600x600px.png.png",
        "price": 37.9,
        "originalPrice": 47.5,
        "deliveryPrice": 2,
        "language": "nl",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      },
      {
        "apiVersion": 1,
        "id": "ddde95a7-dc5f-4a23-82ae-000000000011",
        "slug": "summer-beauty-box",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-image-700x340px.png",
        "thumbnailTitle": "De \"Beauty Box\"\r\n",
        "thumbnailDescription": "Beauty klaar voor elke dag! \r\n",
        "title": "Jouw \"Beauty Box\" voor €26,90  i.p.v. €41,72",
        "description": "\r\nDeze \"Beauty Box\" helpt je om fris de dag door te komen. Van een gezond gebit met Signal tandpasta en mondwater, goede verzorging van de huid dankzij Dove douchegel, deodorant en body lotion tot zachte en verzorgde haren met Timotei shampoo en conditioner. Bestel al deze topproducten nu voor slechts €26,90 i.p.v. €41,72! \r\n\r\n{#alert .alert .alert-warning}\r\nJe ontvangt jouw box maximum 7 werkdagen na jouw bestelling. (verzendkosten = € 2)\r\n\r\n#### Deze \"Beauty Box\" bevat:\r\n- Dove spray deodorant Invisible Dry 150 ml\r\n- Dove Limited edition Douchegel Summer Breeze 250 ml\r\n- Dove Body Lotion Nourishing Body Care SPF 15 250 ml\r\n- Dove Derma Spa Body Mousse Summer Revived Fair 150 ml\r\n- Signal Integral 8 Tandpasta Charcoal witheid\r\n- Signal Mondwater Pro Complete 500 ml\r\n- Timotei Conditioner Pure Nourished & Light 300 ml\r\n- Timotei Pure Shampoo Nourished & light Coconut 300 ml\r\n\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-packshotsAantal-100x100px-8.png)\r\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/beauty-box/myShopiGreatDeal-Unilever-BeautyBox-box-600x600px.png",
        "price": 26.9,
        "originalPrice": 41.72,
        "deliveryPrice": 2,
        "language": "nl",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      },
      {
        "apiVersion": 1,
        "id": "d13e95a7-dc5f-4a23-82ae-f0613f350000",
        "slug": "box-d-drinks-family",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-image-700x340px.png",
        "thumbnailTitle": "De \"D-Drinks Family\" Keuken Box",
        "thumbnailDescription": "Ontdek deze doos samen met heel de familie!",
        "title": "Jouw \"D-Drinks Family\" Keuken Box voor € 43,90 i.p.v. € 50",
        "description": "Ben je klaar om op een grote ontdekkingsreis te gaan met heel de familie? De \"D-Drinks Family\" Keuken Box zit boordevol dranken en snacks waar je elke keer een \"oh dit is lekker en gezonder dan ik gewoon ben\" – momenten mee gaat beleven met het hele gezin.\r\nDe doos bevat het laatste nieuwe in gezondere en natuurlijke dranken, zonder dat deze moeten inboeten aan smaak.\r\nDeel een heerlijke zak popcorn tijdens een apéro of geniet van een frisse limonade na een warme zomerdag, je kan het allemaal terugvinden in een grote, stijlvolle doos. Voor ieder wat wils !\r\nProost !\r\n\r\n{#alert .alert .alert-warning}\r\nJe ontvangt jouw box maximum 7 werkdagen na jouw bestelling. (verzendkosten = € 1)\r\n\r\n#### Deze \"D-Drinks Family\" Keuken Box bevat:\r\n- Green Tea Ginseng & Honey 500ml PET\r\n- Kiwi strawberry 500ml PET\r\n- Raspberries 330ML CAN \r\n- Green Tea Pomegranate 330ML PET\r\n- Juice Shot Turmeric, Ginger & Pineapple 60ML GLASS\r\n- Juice Shot Ginger & Lemon 60ML GLASS\r\n- Reload Lemon & Lime 500ML PET\r\n- Defence Citrus & Elderflower 500ML PET\r\n- 500ml\r\n- Sparkling Ginger 330ML CAN\r\n- Sparkling Apple &Rhubarb 330ML CAN\r\n- KIDS APPLE PEAR 200ML TETRA\r\n- KIDS SUMMER BERRIES 200ML TETRA\r\n- Raspberry 250ML CAN\r\n- Kiwi, Lime & Mint 400ML PET\r\n- MINI 150ML CAN\r\n- Alcohol Free Rhubarb G&T 250ML CAN\r\n- Iced Berry 500ml PET\r\n- Hazelnut & Nougat 55g BAR\r\n- Pop Protein Crisps Sweet Barbecue 28G SINGLE SERVE\r\n- Lovely Sweet 'n Salty Popcorn 35G SINGLE SERVE\r\n- Vanilla & Choc Chip 55G BAR\r\n- Blueberries & Chia Seeds Fruit 45G BAR\r\n- Freeze-Dried Peach 20G BAG\r\n- Sour Flower 100G SHARING BAG\r\n- Hazelnut Crunch 33G SNACKING PACK\r\n\r\n\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-8.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-9.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-10.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-11.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-12.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-13.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-14.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-15.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-16.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-17.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-18.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-19.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-20.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-21.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-22.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-23.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-24.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-25.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-26.png)\r\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-family-box/myShopiGreatDeal-D-Drinks-box-600x600px-v2.png",
        "price": 43.9,
        "originalPrice": 50,
        "deliveryPrice": 1,
        "language": "nl",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      },
      {
        "apiVersion": 1,
        "id": "d13e95a7-dc5f-4a23-82ae-f0613f360001",
        "slug": "box-d-drink-discovery",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-image-700x340px.png",
        "thumbnailTitle": "De \"D-Drinks Discovery\" Keuken Box",
        "thumbnailDescription": "Klaar om mee op ontdekkingsreis te gaan? \r\n",
        "title": "Jouw \"D-Drinks Discovery\" Keuken Box voor € 23,50 i.p.v. € 35\r\n\n",
        "description": "Ben je klaar om op een grote ontdekkingsreis te gaan? De \"D-Drinks Discovery\" Keuken Box zit boordevol dranken en snacks waar je elke keer een “oh dit is lekker en gezonder dan ik gewoon ben” – momenten mee gaat beleven.\r\nDe doos bevat het laatste nieuwe in gezondere en natuurlijke dranken, zonder dat deze moeten inboeten aan smaak.\r\nDeel een heerlijke zak popcorn tijdens een apéro of geniet van een frisse limonade na een warme zomerdag, je kan het allemaal terugvinden in een grote, stijlvolle doos. Voor ieder wat wils !\r\nProost !\r\n\r\n{#alert .alert .alert-warning}\r\nJe ontvangt jouw box maximum 7 werkdagen na jouw bestelling. (verzendkosten = € 1)\r\n\r\n\r\n#### Deze \"D-Drinks Dicovery\" Keuken Box bevat:\r\n- Green Tea Ginseng & Honey 330ML CAN\r\n- Green Tea Pomegranate 330ML PET\r\n- Juice Shot Ginger & Lemon 60ML GLASS\r\n- Reload Lemon & Lime 500ML PET\r\n- Sparkling Ginger 330ML CAN\r\n- KIDS APPLE PEAR 200ML TETRA\r\n- KIDS SUMMER BERRIES 200ML TETRA\r\n- Raspberry 250ML CAN\r\n- MINI 150ML CAN\r\n- Natural Energy 250ML CAN\r\n- Apple Kiwi 500ml PET\r\n- Hazelnut & Nougat 55g BAR\r\n- Pop Corn Crisps Sea Salt 28G SINGLE SERVE\r\n- Lovely Sweet 'n Salty Popcorn 35G SINGLE SERVE\r\n- Vanilla & Choc Chip 55G BAR\r\n- Hazelnut Crunch 33G SNACKING PACK\r\n\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-8.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-9.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-10.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-11.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-12.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-13.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-14.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-15.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-packshotsAantal-100x100px-16.png)\r\n\r\n\r\n",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/d-drinks-discovery-box/myShopiGreatDeal-D-Drinks-box-600x600px-v2.png",
        "price": 23.5,
        "originalPrice": 35,
        "deliveryPrice": 1,
        "language": "nl",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      },
      {
        "apiVersion": 1,
        "id": "00000077-dc5f-4a23-82ae-f0613f3c9ba7",
        "slug": "food-of-the-world-box",
        "image": "https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-image-700x340px.png",
        "thumbnailTitle": "\"Food of the World\" Keuken Box\r\n",
        "thumbnailDescription": "Laat je inspireren door de wereldkeuken!\r\n",
        "title": "Jouw \"Food of the World\" Keuken Box voor € 29,99 i.p.v. € 51",
        "description": "Geen inspiratie om te koken? En wil je het vakantiegevoel naar je keuken brengen? Wat dacht je van Mexicaanse taco's, Thaise curry, Indiaase tikka masala of Hawaiaanse poké bowls? Met deze \"Food of the World\" Keuken Box tover jij de heerlijkste gerechten op jouw bord en haal je de wereld naar jouw keuken. Naast de ingrediënten voor deze topgerechten, vind je in jouw \"Food of the World\"  Keuken Box ook 4 receptkaartjes. Hierin wordt in enkele eenvoudige stappen uitgelegd hoe je deze maaltijd in slechts 25 minuten klaarmaakt. Smakelijk!\r\n\r\n{#alert .alert .alert-warning}\r\nJe ontvangt jouw box maximum 7 werkdagen na jouw bestelling. (verzendkosten = € 2)\r\n\r\n\r\n#### Deze \"Food of the World\" Keuken Box bevat:\r\n- Suzi Wan kokosmelk (500 ml)\r\n- Suzi Wan Sambal Oelek (100 gr)\r\n- Suzi Wan nest noedels (250 gr)\r\n- Sharwoods Tikka Masala saus (420 gr)\r\n- Sharwoods Poppadoms (pak van 8)\r\n- Sharwoods Green Label Mango Chutney (227 gr)\r\n- Blue Dragon sushi rijst (500 gr)\r\n- Blue Dragon rijstazijn (150 ml)\r\n- Blue Dragon Japanse sojasaus (150 ml)\r\n- Blue Dragon sushi gember (145 gr)\r\n- Old El Paso taco shells (pak van 12)\r\n- Old El Paso sliced green Jalapeños (215 gr)\r\n- Old El Paso kruidenmengsel voor taco's (25 gr)\r\n- Old El Paso taco salsa (235 gr)\r\n\r\n\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-1.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-2.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-3.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-4.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-5.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-6.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-7.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-8.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-9.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-10.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-11.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-12.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-13.png)\r\n![](https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-packshotsAantal-100x100px-14.png)",
        "thumbnailImage": "https://agilysimages.blob.core.windows.net/websites/great-deals/food-of-the-world-box/myShopiGreatDeal-Pietercil-box-600x600px-NL.png",
        "price": 29.99,
        "originalPrice": 51,
        "deliveryPrice": 2,
        "language": "nl",
        "category": "non-food",
        "status": "SOLDOUT",
        "forAdults": false,
        "bannerText": "",
        "bannerColor": ""
      }
    ]
  }
}

```
