<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Inutilepedia - Le Savoir Absolument Indispensable</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f2f5;
            color: #333;
        }
        .container {
            max-width: 800px;
        }
        .fact-card {
            background-color: #ffffff;
            border: 1px solid #e2e8f0;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .progress-bar-container {
            background-color: #e0e0e0;
        }
        .progress-bar-fill {
            background-color: #6366f1; /* Indigo 500 */
            transition: width 0.5s ease-in-out;
        }
        .btn-primary {
            background-color: #4f46e5; /* Indigo 600 */
            color: white;
            padding: 0.75rem 1.5rem;
            border-radius: 0.5rem;
            font-weight: 600;
            transition: background-color 0.2s ease-in-out, transform 0.1s ease-in-out;
        }
        .btn-primary:hover {
            background-color: #4338ca; /* Indigo 700 */
            transform: translateY(-1px);
        }
        .btn-secondary {
            background-color: #e0e7ff; /* Indigo 100 */
            color: #4f46e5; /* Indigo 600 */
            padding: 0.5rem 1rem;
            border-radius: 0.5rem;
            font-weight: 600;
            transition: background-color 0.2s ease-in-out;
        }
        .btn-secondary:hover {
            background-color: #c7d2fe; /* Indigo 200 */
        }
        .btn-vote {
            padding: 0.5rem 1rem;
            border-radius: 0.5rem;
            font-weight: 600;
            transition: background-color 0.2s ease-in-out;
        }
        .btn-vote.yes {
            background-color: #22c55e; /* Green 500 */
            color: white;
        }
        .btn-vote.yes:hover {
            background-color: #16a34a; /* Green 600 */
        }
        .btn-vote.no {
            background-color: #ef4444; /* Red 500 */
            color: white;
        }
        .btn-vote.no:hover {
            background-color: #dc2626; /* Red 600 */
        }
        .category-btn {
            background-color: #e0e7ff; /* Indigo 100 */
            color: #4f46e5; /* Indigo 600 */
            padding: 0.4rem 0.8rem;
            border-radius: 9999px; /* Full rounded */
            font-size: 0.875rem; /* text-sm */
            font-weight: 500;
            transition: background-color 0.2s ease-in-out;
            cursor: default; /* Not clickable for now */
        }
        .category-btn:hover {
            background-color: #c7d2fe; /* Indigo 200 */
        }
        .category-btn.active {
            background-color: #4f46e5; /* Indigo 600 */
            color: white;
        }
        .message-box {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #333;
            color: white;
            padding: 10px 20px;
            border-radius: 8px;
            opacity: 0;
            transition: opacity 0.3s ease-in-out;
            z-index: 1000;
        }
        .message-box.show {
            opacity: 1;
        }
    </style>
</head>
<body class="flex flex-col min-h-screen items-center py-8 px-4">

    <div class="container mx-auto p-6 bg-white rounded-xl shadow-lg mb-8">
        <h1 class="text-5xl font-extrabold text-center text-indigo-700 mb-6">Inutilepedia</h1>
        <p class="text-center text-lg text-gray-600 mb-10">
            Le savoir absolument indispensable dont personne n'a besoin.
        </p>

        <div id="fact-display" class="fact-card p-6 rounded-lg text-center mb-8">
            <p id="current-fact" class="text-2xl font-semibold mb-4 text-gray-800">
                Cliquez sur le bouton pour découvrir un fait inutile !
            </p>
            <div class="flex justify-center items-center gap-2 text-gray-500 text-sm">
                <span id="fact-category" class="category-btn"></span>
            </div>
        </div>

        <div class="flex justify-center mb-8">
            <button id="new-fact-btn" class="btn-primary text-xl">
                <i class="fas fa-lightbulb mr-2"></i> Donne-moi un fait inutile !
            </button>
        </div>

        <div class="mb-8">
            <h3 class="text-xl font-semibold text-center text-gray-700 mb-3">Niveau d'inutilité :</h3>
            <div class="progress-bar-container w-full h-6 rounded-full overflow-hidden">
                <div id="inutility-progress" class="progress-bar-fill h-full rounded-full" style="width: 0%;"></div>
            </div>
            <p id="inutility-text" class="text-center text-sm text-gray-500 mt-2">0% d'inutilité</p>
        </div>

        <div class="text-center mb-8">
            <h3 class="text-xl font-semibold text-gray-700 mb-3">Est-ce que c'était utile ?</h3>
            <div class="flex justify-center gap-4">
                <button id="vote-yes-btn" class="btn-vote yes">
                    <i class="fas fa-check mr-2"></i> Oui
                </button>
                <button id="vote-no-btn" class="btn-vote no">
                    <i class="fas fa-times mr-2"></i> Non
                </button>
            </div>
            <p class="text-sm text-gray-500 mt-3">
                Utile : <span id="useful-count">0</span> | Inutile : <span id="useless-count">0</span>
            </p>
            <p class="text-xs text-gray-400 mt-1">
                (Ces statistiques sont réinitialisées à chaque session)
            </p>
        </div>

        <div class="text-center mb-10">
            <button id="share-fact-btn" class="btn-secondary">
                <i class="fas fa-share-alt mr-2"></i> Partage à un pote qui a besoin de savoir ça
            </button>
        </div>

        <div class="fact-card p-6 rounded-lg mb-8">
            <h2 class="text-2xl font-semibold text-center text-indigo-700 mb-4">Soumettre un Fait Inutile</h2>
            <p class="text-gray-600 text-center mb-4">
                Vous avez découvert une perle d'inutilité ? Partagez-la avec le monde !
            </p>
            <textarea id="submit-fact-text"
                      class="w-full p-3 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500 mb-4"
                      rows="4"
                      placeholder="Tapez votre fait inutile ici..."></textarea>
            <div class="flex justify-center">
                <button id="submit-fact-btn" class="btn-primary">
                    <i class="fas fa-paper-plane mr-2"></i> Soumettre le fait
                </button>
            </div>
            <p class="text-sm text-gray-500 mt-3 text-center">
                (Les faits soumis seront modérés avant d'apparaître sur Inutilepedia.)
            </p>
        </div>
    </div>

    <div id="message-box" class="message-box"></div>

    <script>
        // Tableau des faits inutiles (beaucoup plus grand !)
        const uselessFacts = [
            { text: "Les canards ont des accents régionaux, et les chercheurs peuvent les distinguer.", category: "Animaux", inutilityLevel: 70 },
            { text: "Il est scientifiquement impossible de se chatouiller soi-même à cause de la façon dont votre cerveau anticipe vos propres mouvements.", category: "Corps Humain", inutilityLevel: 90 },
            { text: "Le 'cri du homard' est en réalité le bruit de l'air et de la vapeur s'échappant de sa carapace pendant la cuisson.", category: "Sciences", inutilityLevel: 60 },
            { text: "Au Texas, il est illégal de posséder plus de six vibromasseurs. Toute personne en ayant sept ou plus peut être considérée comme un distributeur commercial.", category: "Lois Débiles", inutilityLevel: 95 },
            { text: "Un humain produit en moyenne assez de salive en un an pour remplir deux baignoires de taille normale.", category: "Corps Humain", inutilityLevel: 85 },
            { text: "Les crocodiles ne peuvent pas tirer leur langue.", category: "Animaux", inutilityLevel: 50 },
            { text: "Les chameaux ont trois paupières pour se protéger du sable.", category: "Animaux", inutilityLevel: 40 },
            { text: "La Tour Eiffel peut grandir jusqu'à 15 cm en été à cause de la dilatation thermique du métal.", category: "Sciences", inutilityLevel: 75 },
            { text: "Les astronautes ne peuvent pas roter dans l'espace.", category: "Sciences", inutilityLevel: 80 },
            { text: "Le cœur d'une crevette est situé dans sa tête.", category: "Animaux", inutilityLevel: 65 },
            { text: "Les éléphants sont les seuls animaux qui ne peuvent pas sauter.", category: "Animaux", inutilityLevel: 70 },
            { text: "Le 'baiser français' était autrefois appelé 'baiser florentin' en Angleterre.", category: "Histoire", inutilityLevel: 55 },
            { text: "La langue de la baleine bleue pèse autant qu'un éléphant.", category: "Animaux", inutilityLevel: 78 },
            { text: "Il n'y a pas de moutarde dans l'eau de Javel.", category: "Culture Pop", inutilityLevel: 45 },
            { text: "Les koalas dorment environ 20 heures par jour.", category: "Animaux", inutilityLevel: 60 },
            { text: "Les fourmis ne dorment jamais.", category: "Animaux", inutilityLevel: 50 },
            { text: "Un seul spaghetti est appelé un spaghetto.", category: "Culture Pop", inutilityLevel: 72 },
            { text: "Les vers de terre ont cinq cœurs.", category: "Animaux", inutilityLevel: 68 },
            { text: "Le son le plus fort jamais enregistré a été le Krakatoa en 1883, audible à 4 800 km.", category: "Histoire", inutilityLevel: 88 },
            { text: "Les cochons ne peuvent pas regarder le ciel.", category: "Animaux", inutilityLevel: 75 },
            { text: "Le nom complet de la ville de Los Angeles est 'El Pueblo de Nuestra Señora la Reina de los Ángeles de Porciúncula'.", category: "Histoire", inutilityLevel: 92 },
            { text: "Les étoiles de mer n'ont pas de cerveau.", category: "Animaux", inutilityLevel: 58 },
            { text: "La plupart des rouges à lèvres contiennent des écailles de poisson.", category: "Culture Pop", inutilityLevel: 83 },
            { text: "Les cacahuètes sont des légumineuses, pas des noix.", category: "Sciences", inutilityLevel: 62 },
            { text: "Les flamants roses ne sont roses que parce qu'ils mangent des crevettes roses.", category: "Animaux", inutilityLevel: 70 },
            { text: "Les chats peuvent faire plus de 100 sons différents, tandis que les chiens n'en font qu'environ 10.", category: "Animaux", inutilityLevel: 55 },
            { text: "Le cri d'un paon est si fort qu'il peut endommager l'ouïe humaine à proximité.", category: "Animaux", inutilityLevel: 77 },
            { text: "Les otaries peuvent danser en rythme avec la musique.", category: "Animaux", inutilityLevel: 80 },
            { text: "Le plus long mot en anglais sans voyelle est 'rhythms'.", category: "Culture Pop", inutilityLevel: 85 },
            { text: "Le miel est le seul aliment qui ne se gâte jamais.", category: "Sciences", inutilityLevel: 65 },
            { text: "Les vaches ont un meilleur ami et deviennent stressées si elles en sont séparées.", category: "Animaux", inutilityLevel: 73 },
            { text: "Les manchots n'ont pas de genoux.", category: "Animaux", inutilityLevel: 68 },
            { text: "Le point sur le 'i' s'appelle un 'tittle'.", category: "Culture Pop", inutilityLevel: 88 },
            { text: "Les requins sont les seuls poissons qui peuvent cligner des yeux.", category: "Animaux", inutilityLevel: 79 },
            { text: "Il y a plus de six fois plus de poulets que d'humains dans le monde.", category: "Animaux", inutilityLevel: 60 },
            { text: "Les rats peuvent rire quand ils sont chatouillés.", category: "Animaux", inutilityLevel: 82 },
            { text: "Le sang des limules est bleu.", category: "Sciences", inutilityLevel: 74 },
            { text: "Les chaussettes ont été inventées pour protéger les pieds.", category: "Histoire", inutilityLevel: 30 },
            { text: "Les pommes flottent parce qu'elles sont composées à 25% d'air.", category: "Sciences", inutilityLevel: 67 },
            { text: "Les dauphins dorment avec un œil ouvert.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les mouches domestiques vivent en moyenne 15 à 30 jours.", category: "Animaux", inutilityLevel: 45 },
            { text: "Les serpents peuvent entendre à travers leurs mâchoires.", category: "Animaux", inutilityLevel: 81 },
            { text: "La plupart des gens ont peur des araignées, mais les araignées ont plus peur des humains.", category: "Animaux", inutilityLevel: 76 },
            { text: "Les lamas sont très sélectifs et ne crachent que sur les personnes qu'ils n'aiment pas.", category: "Animaux", inutilityLevel: 89 },
            { text: "Le son d'un pet de baleine bleue est plus fort qu'un moteur d'avion.", category: "Animaux", inutilityLevel: 93 },
            { text: "Les paresseux sont si lents que des algues poussent sur leur fourrure.", category: "Animaux", inutilityLevel: 91 },
            { text: "Les kangourous ne peuvent pas sauter en arrière.", category: "Animaux", inutilityLevel: 72 },
            { text: "Les fourmis peuvent soulever 50 fois leur propre poids.", category: "Animaux", inutilityLevel: 69 },
            { text: "Les araignées sont capables de voler en utilisant de l'électricité statique.", category: "Sciences", inutilityLevel: 87 },
            { text: "Le cerveau humain est plus actif la nuit que le jour.", category: "Corps Humain", inutilityLevel: 55 },
            { text: "Un éclair est cinq fois plus chaud que la surface du soleil.", category: "Sciences", inutilityLevel: 78 },
            { text: "Les poulpes ont trois cœurs.", category: "Animaux", inutilityLevel: 84 },
            { text: "Les chèvres ont des pupilles rectangulaires.", category: "Animaux", inutilityLevel: 86 },
            { text: "La durée de vie moyenne d'un globule rouge est de 120 jours.", category: "Corps Humain", inutilityLevel: 40 },
            { text: "Il faut environ 8 minutes et 20 secondes à la lumière du soleil pour atteindre la Terre.", category: "Sciences", inutilityLevel: 63 },
            { text: "Le cri d'un canard ne produit pas d'écho.", category: "Animaux", inutilityLevel: 94 },
            { text: "Les chats ne peuvent pas goûter le sucre.", category: "Animaux", inutilityLevel: 70 },
            { text: "Les hiboux sont les seuls oiseaux qui peuvent voir la couleur bleue.", category: "Animaux", inutilityLevel: 75 },
            { text: "Les cochons d'Inde ne sont pas des cochons et ne viennent pas de Guinée.", category: "Animaux", inutilityLevel: 80 },
            { text: "Les autruches ont des yeux plus grands que leur cerveau.", category: "Animaux", inutilityLevel: 90 },
            { text: "Les ours polaires ont la peau noire sous leur fourrure blanche.", category: "Animaux", inutilityLevel: 65 },
            { text: "La plus grande pizza jamais faite mesurait 37,8 mètres de diamètre.", category: "Culture Pop", inutilityLevel: 88 },
            { text: "Les humains partagent 50% de leur ADN avec une banane.", category: "Sciences", inutilityLevel: 92 },
            { text: "Le mot 'nerd' a été inventé par Dr. Seuss.", category: "Culture Pop", inutilityLevel: 77 },
            { text: "Les abeilles peuvent être entraînées à détecter des explosifs.", category: "Animaux", inutilityLevel: 81 },
            { text: "Le cri du dodo était si fort qu'il pouvait être entendu à des kilomètres.", category: "Histoire", inutilityLevel: 85 },
            { text: "Les vombats produisent des excréments cubiques.", category: "Animaux", inutilityLevel: 98 },
            { text: "Les papillons goûtent avec leurs pieds.", category: "Animaux", inutilityLevel: 73 },
            { text: "Les girafes se nettoient les oreilles avec leur langue de 50 cm.", category: "Animaux", inutilityLevel: 89 },
            { text: "Les étoiles de mer peuvent régénérer des membres perdus.", category: "Animaux", inutilityLevel: 60 },
            { text: "Le nom 'Google' est une faute d'orthographe de 'googol', un nombre qui est 1 suivi de 100 zéros.", category: "Culture Pop", inutilityLevel: 75 },
            { text: "Les caméléons peuvent bouger leurs yeux dans des directions différentes indépendamment.", category: "Animaux", inutilityLevel: 79 },
            { text: "Le plus ancien chewing-gum a 9 000 ans.", category: "Histoire", inutilityLevel: 68 },
            { text: "Les moustiques sont attirés par les personnes qui viennent de manger des bananes.", category: "Animaux", inutilityLevel: 82 },
            { text: "Les poulets peuvent reconnaître plus de 100 visages différents.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les lapins et les perroquets peuvent voir derrière eux sans bouger la tête.", category: "Animaux", inutilityLevel: 74 },
            { text: "Le plus long mot en français sans 'e' est 'constitutionnellement'.", category: "Culture Pop", inutilityLevel: 87 },
            { text: "Une personne moyenne passera six mois de sa vie à attendre aux feux rouges.", category: "Corps Humain", inutilityLevel: 90 },
            { text: "Les cafards peuvent vivre plusieurs semaines sans tête.", category: "Animaux", inutilityLevel: 95 },
            { text: "Les pingouins ont une glande spéciale qui leur permet de boire de l'eau salée.", category: "Animaux", inutilityLevel: 65 },
            { text: "Les grains de maïs éclatés sont appelés 'popcorn' car ils éclatent en chauffant.", category: "Culture Pop", inutilityLevel: 40 },
            { text: "Les serpents peuvent survivre pendant deux ans sans manger.", category: "Animaux", inutilityLevel: 83 },
            { text: "La couleur orange a été nommée d'après le fruit, pas l'inverse.", category: "Culture Pop", inutilityLevel: 70 },
            { text: "Les escargots ont des milliers de dents microscopiques sur leur langue.", category: "Animaux", inutilityLevel: 78 },
            { text: "Les poissons rouges ont une mémoire de trois secondes.", category: "Animaux", inutilityLevel: 50 },
            { text: "Le saviez-vous ? Le mot 'trivia' vient du latin 'trivium', signifiant 'trois chemins', où des informations banales étaient partagées.", category: "Histoire", inutilityLevel: 80 },
            { text: "Les chats passent 70% de leur vie à dormir.", category: "Animaux", inutilityLevel: 60 },
            { text: "Les hippopotames peuvent courir plus vite que les humains.", category: "Animaux", inutilityLevel: 67 },
            { text: "Les chauves-souris sont les seuls mammifères capables de voler.", category: "Animaux", inutilityLevel: 55 },
            { text: "Les pommes de terre sont composées à 80% d'eau.", category: "Sciences", inutilityLevel: 45 },
            { text: "Les lapins mangent leurs propres excréments pour obtenir plus de nutriments.", category: "Animaux", inutilityLevel: 91 },
            { text: "Le bruit que vous entendez lorsque vous craquez vos doigts est le gaz qui s'échappe de vos articulations.", category: "Corps Humain", inutilityLevel: 72 },
            { text: "Les yeux d'une autruche sont plus grands que son cerveau.", category: "Animaux", inutilityLevel: 85 },
            { text: "Les chats ne peuvent pas voir leur nez.", category: "Animaux", inutilityLevel: 79 },
            { text: "Les abeilles meurent après avoir piqué.", category: "Animaux", inutilityLevel: 68 },
            { text: "Les ours peuvent courir aussi vite que les chevaux.", category: "Animaux", inutilityLevel: 74 },
            { text: "Le cerveau d'Einstein a été volé après sa mort.", category: "Histoire", inutilityLevel: 93 },
            { text: "Les toilettes ont été inventées avant le papier toilette.", category: "Histoire", inutilityLevel: 88 },
            { text: "Les zèbres sont noirs avec des rayures blanches, pas l'inverse.", category: "Animaux", inutilityLevel: 82 },
            { text: "Les escargots peuvent dormir pendant trois ans.", category: "Animaux", inutilityLevel: 90 },
            { text: "Le cœur d'une baleine bleue bat seulement 6 fois par minute.", category: "Animaux", inutilityLevel: 77 },
            { text: "Les humains sont les seuls animaux à pleurer des larmes émotionnelles.", category: "Corps Humain", inutilityLevel: 65 },
            { text: "La seule nourriture qui ne se gâte jamais est le miel.", category: "Sciences", inutilityLevel: 60 },
            { text: "Les étoiles de mer n'ont pas de sang.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les phoques ont des moustaches aussi sensibles que les empreintes digitales humaines.", category: "Animaux", inutilityLevel: 84 },
            { text: "Le bruit d'un éclair est le son de l'air qui se dilate rapidement.", category: "Sciences", inutilityLevel: 58 },
            { text: "Les caméléons changent de couleur en fonction de leur humeur, pas seulement de leur environnement.", category: "Animaux", inutilityLevel: 86 },
            { text: "Les fourmis sont capables de créer des ponts vivants pour traverser des obstacles.", category: "Animaux", inutilityLevel: 75 },
            { text: "Les poissons rouges peuvent vivre jusqu'à 40 ans s'ils sont bien soignés.", category: "Animaux", inutilityLevel: 62 },
            { text: "Les pandas géants mangent environ 12 heures par jour.", category: "Animaux", inutilityLevel: 55 },
            { text: "Le plus grand iceberg jamais enregistré était plus grand que la Belgique.", category: "Sciences", inutilityLevel: 89 },
            { text: "Les bébés naissent avec 300 os, mais les adultes n'en ont que 206.", category: "Corps Humain", inutilityLevel: 70 },
            { text: "Les chevaux ne peuvent pas vomir.", category: "Animaux", inutilityLevel: 73 },
            { text: "Les serpents ont des paupières transparentes qui restent fermées.", category: "Animaux", inutilityLevel: 76 },
            { text: "Les requins n'ont pas d'os, leur squelette est fait de cartilage.", category: "Animaux", inutilityLevel: 69 },
            { text: "Les chauves-souris utilisent l'écholocation pour naviguer dans l'obscurité.", category: "Animaux", inutilityLevel: 64 },
            { text: "Les humains ont plus de bactéries dans leur bouche que d'habitants sur Terre.", category: "Corps Humain", inutilityLevel: 94 },
            { text: "Les chèvres ont un accent.", category: "Animaux", inutilityLevel: 96 },
            { text: "Il y a plus de six fois plus de poulets que d'humains dans le monde.", category: "Animaux", inutilityLevel: 60 },
            { text: "Les rats peuvent rire quand ils sont chatouillés.", category: "Animaux", inutilityLevel: 82 },
            { text: "Le sang des limules est bleu.", category: "Sciences", inutilityLevel: 74 },
            { text: "Les chaussettes ont été inventées pour protéger les pieds.", category: "Histoire", inutilityLevel: 30 },
            { text: "Les pommes flottent parce qu'elles sont composées à 25% d'air.", category: "Sciences", inutilityLevel: 67 },
            { text: "Les dauphins dorment avec un œil ouvert.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les mouches domestiques vivent en moyenne 15 à 30 jours.", category: "Animaux", inutilityLevel: 45 },
            { text: "Les serpents peuvent entendre à travers leurs mâchoires.", category: "Animaux", inutilityLevel: 81 },
            { text: "La plupart des gens ont peur des araignées, mais les araignées ont plus peur des humains.", category: "Animaux", inutilityLevel: 76 },
            { text: "Les lamas sont très sélectifs et ne crachent que sur les personnes qu'ils n'aiment pas.", category: "Animaux", inutilityLevel: 89 },
            { text: "Le son d'un pet de baleine bleue est plus fort qu'un moteur d'avion.", category: "Animaux", inutilityLevel: 93 },
            { text: "Les paresseux sont si lents que des algues poussent sur leur fourrure.", category: "Animaux", inutilityLevel: 91 },
            { text: "Les kangourous ne peuvent pas sauter en arrière.", category: "Animaux", inutilityLevel: 72 },
            { text: "Les fourmis peuvent soulever 50 fois leur propre poids.", category: "Animaux", inutilityLevel: 69 },
            { text: "Les araignées sont capables de voler en utilisant de l'électricité statique.", category: "Sciences", inutilityLevel: 87 },
            { text: "Le cerveau humain est plus actif la nuit que le jour.", category: "Corps Humain", inutilityLevel: 55 },
            { text: "Un éclair est cinq fois plus chaud que la surface du soleil.", category: "Sciences", inutilityLevel: 78 },
            { text: "Les poulpes ont trois cœurs.", category: "Animaux", inutilityLevel: 84 },
            { text: "Les chèvres ont des pupilles rectangulaires.", category: "Animaux", inutilityLevel: 86 },
            { text: "La durée de vie moyenne d'un globule rouge est de 120 jours.", category: "Corps Humain", inutilityLevel: 40 },
            { text: "Il faut environ 8 minutes et 20 secondes à la lumière du soleil pour atteindre la Terre.", category: "Sciences", inutilityLevel: 63 },
            { text: "Le cri d'un canard ne produit pas d'écho.", category: "Animaux", inutilityLevel: 94 },
            { text: "Les chats ne peuvent pas goûter le sucre.", category: "Animaux", inutilityLevel: 70 },
            { text: "Les hiboux sont les seuls oiseaux qui peuvent voir la couleur bleue.", category: "Animaux", inutilityLevel: 75 },
            { text: "Les cochons d'Inde ne sont pas des cochons et ne viennent pas de Guinée.", category: "Animaux", inutilityLevel: 80 },
            { text: "Les autruches ont des yeux plus grands que leur cerveau.", category: "Animaux", inutilityLevel: 90 },
            { text: "Les ours polaires ont la peau noire sous leur fourrure blanche.", category: "Animaux", inutilityLevel: 65 },
            { text: "La plus grande pizza jamais faite mesurait 37,8 mètres de diamètre.", category: "Culture Pop", inutilityLevel: 88 },
            { text: "Les humains partagent 50% de leur ADN avec une banane.", category: "Sciences", inutilityLevel: 92 },
            { text: "Le mot 'nerd' a été inventé par Dr. Seuss.", category: "Culture Pop", inutilityLevel: 77 },
            { text: "Les abeilles peuvent être entraînées à détecter des explosifs.", category: "Animaux", inutilityLevel: 81 },
            { text: "Le cri du dodo était si fort qu'il pouvait être entendu à des kilomètres.", category: "Histoire", inutilityLevel: 85 },
            { text: "Les vombats produisent des excréments cubiques.", category: "Animaux", inutilityLevel: 98 },
            { text: "Les papillons goûtent avec leurs pieds.", category: "Animaux", inutilityLevel: 73 },
            { text: "Les girafes se nettoient les oreilles avec leur langue de 50 cm.", category: "Animaux", inutilityLevel: 89 },
            { text: "Les étoiles de mer peuvent régénérer des membres perdus.", category: "Animaux", inutilityLevel: 60 },
            { text: "Le nom 'Google' est une faute d'orthographe de 'googol', un nombre qui est 1 suivi de 100 zéros.", category: "Culture Pop", inutilityLevel: 75 },
            { text: "Les caméléons peuvent bouger leurs yeux dans des directions différentes indépendamment.", category: "Animaux", inutilityLevel: 79 },
            { text: "Le plus ancien chewing-gum a 9 000 ans.", category: "Histoire", inutilityLevel: 68 },
            { text: "Les moustiques sont attirés par les personnes qui viennent de manger des bananes.", category: "Animaux", inutilityLevel: 82 },
            { text: "Les poulets peuvent reconnaître plus de 100 visages différents.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les lapins et les perroquets peuvent voir derrière eux sans bouger la tête.", category: "Animaux", inutilityLevel: 74 },
            { text: "Le plus long mot en français sans 'e' est 'constitutionnellement'.", category: "Culture Pop", inutilityLevel: 87 },
            { text: "Une personne moyenne passera six mois de sa vie à attendre aux feux rouges.", category: "Corps Humain", inutilityLevel: 90 },
            { text: "Les cafards peuvent vivre plusieurs semaines sans tête.", category: "Animaux", inutilityLevel: 95 },
            { text: "Les pingouins ont une glande spéciale qui leur permet de boire de l'eau salée.", category: "Animaux", inutilityLevel: 65 },
            { text: "Les grains de maïs éclatés sont appelés 'popcorn' car ils éclatent en chauffant.", category: "Culture Pop", inutilityLevel: 40 },
            { text: "Les serpents peuvent survivre pendant deux ans sans manger.", category: "Animaux", inutilityLevel: 83 },
            { text: "La couleur orange a été nommée d'après le fruit, pas l'inverse.", category: "Culture Pop", inutilityLevel: 70 },
            { text: "Les escargots ont des milliers de dents microscopiques sur leur langue.", category: "Animaux", inutilityLevel: 78 },
            { text: "Les poissons rouges ont une mémoire de trois secondes.", category: "Animaux", inutilityLevel: 50 },
            { text: "Le saviez-vous ? Le mot 'trivia' vient du latin 'trivium', signifiant 'trois chemins', où des informations banales étaient partagées.", category: "Histoire", inutilityLevel: 80 },
            { text: "Les chats passent 70% de leur vie à dormir.", category: "Animaux", inutilityLevel: 60 },
            { text: "Les hippopotames peuvent courir plus vite que les humains.", category: "Animaux", inutilityLevel: 67 },
            { text: "Les chauves-souris sont les seuls mammifères capables de voler.", category: "Animaux", inutilityLevel: 55 },
            { text: "Les pommes de terre sont composées à 80% d'eau.", category: "Sciences", inutilityLevel: 45 },
            { text: "Les lapins mangent leurs propres excréments pour obtenir plus de nutriments.", category: "Animaux", inutilityLevel: 91 },
            { text: "Le bruit que vous entendez lorsque vous craquez vos doigts est le gaz qui s'échappe de vos articulations.", category: "Corps Humain", inutilityLevel: 72 },
            { text: "Les yeux d'une autruche sont plus grands que son cerveau.", category: "Animaux", inutilityLevel: 85 },
            { text: "Les chats ne peuvent pas voir leur nez.", category: "Animaux", inutilityLevel: 79 },
            { text: "Les abeilles meurent après avoir piqué.", category: "Animaux", inutilityLevel: 68 },
            { text: "Les ours peuvent courir aussi vite que les chevaux.", category: "Animaux", inutilityLevel: 74 },
            { text: "Le cerveau d'Einstein a été volé après sa mort.", category: "Histoire", inutilityLevel: 93 },
            { text: "Les toilettes ont été inventées avant le papier toilette.", category: "Histoire", inutilityLevel: 88 },
            { text: "Les zèbres sont noirs avec des rayures blanches, pas l'inverse.", category: "Animaux", inutilityLevel: 82 },
            { text: "Les escargots peuvent dormir pendant trois ans.", category: "Animaux", inutilityLevel: 90 },
            { text: "Le cœur d'une baleine bleue bat seulement 6 fois par minute.", category: "Animaux", inutilityLevel: 77 },
            { text: "Les humains sont les seuls animaux à pleurer des larmes émotionnelles.", category: "Corps Humain", inutilityLevel: 65 },
            { text: "La seule nourriture qui ne se gâte jamais est le miel.", category: "Sciences", inutilityLevel: 60 },
            { text: "Les étoiles de mer n'ont pas de sang.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les phoques ont des moustaches aussi sensibles que les empreintes digitales humaines.", category: "Animaux", inutilityLevel: 84 },
            { text: "Le bruit d'un éclair est le son de l'air qui se dilate rapidement.", category: "Sciences", inutilityLevel: 58 },
            { text: "Les caméléons changent de couleur en fonction de leur humeur, pas seulement de leur environnement.", category: "Animaux", inutilityLevel: 86 },
            { text: "Les fourmis sont capables de créer des ponts vivants pour traverser des obstacles.", category: "Animaux", inutilityLevel: 75 },
            { text: "Les poissons rouges peuvent vivre jusqu'à 40 ans s'ils sont bien soignés.", category: "Animaux", inutilityLevel: 62 },
            { text: "Les pandas géants mangent environ 12 heures par jour.", category: "Animaux", inutilityLevel: 55 },
            { text: "Le plus grand iceberg jamais enregistré était plus grand que la Belgique.", category: "Sciences", inutilityLevel: 89 },
            { text: "Les bébés naissent avec 300 os, mais les adultes n'en ont que 206.", category: "Corps Humain", inutilityLevel: 70 },
            { text: "Les chevaux ne peuvent pas vomir.", category: "Animaux", inutilityLevel: 73 },
            { text: "Les serpents ont des paupières transparentes qui restent fermées.", category: "Animaux", inutilityLevel: 76 },
            { text: "Les requins n'ont pas d'os, leur squelette est fait de cartilage.", category: "Animaux", inutilityLevel: 69 },
            { text: "Les chauves-souris utilisent l'écholocation pour naviguer dans l'obscurité.", category: "Animaux", inutilityLevel: 64 },
            { text: "Les humains ont plus de bactéries dans leur bouche que d'habitants sur Terre.", category: "Corps Humain", inutilityLevel: 94 },
            { text: "Les chèvres ont un accent.", category: "Animaux", inutilityLevel: 96 },
            { text: "Les chats peuvent faire plus de 100 sons différents, tandis que les chiens n'en font qu'environ 10.", category: "Animaux", inutilityLevel: 55 },
            { text: "Le cri d'un paon est si fort qu'il peut endommager l'ouïe humaine à proximité.", category: "Animaux", inutilityLevel: 77 },
            { text: "Les otaries peuvent danser en rythme avec la musique.", category: "Animaux", inutilityLevel: 80 },
            { text: "Le plus long mot en anglais sans voyelle est 'rhythms'.", category: "Culture Pop", inutilityLevel: 85 },
            { text: "Le miel est le seul aliment qui ne se gâte jamais.", category: "Sciences", inutilityLevel: 65 },
            { text: "Les vaches ont un meilleur ami et deviennent stressées si elles en sont séparées.", category: "Animaux", inutilityLevel: 73 },
            { text: "Les manchots n'ont pas de genoux.", category: "Animaux", inutilityLevel: 68 },
            { text: "Le point sur le 'i' s'appelle un 'tittle'.", category: "Culture Pop", inutilityLevel: 88 },
            { text: "Les requins sont les seuls poissons qui peuvent cligner des yeux.", category: "Animaux", inutilityLevel: 79 },
            { text: "Il y a plus de six fois plus de poulets que d'humains dans le monde.", category: "Animaux", inutilityLevel: 60 },
            { text: "Les rats peuvent rire quand ils sont chatouillés.", category: "Animaux", inutilityLevel: 82 },
            { text: "Le sang des limules est bleu.", category: "Sciences", inutilityLevel: 74 },
            { text: "Les chaussettes ont été inventées pour protéger les pieds.", category: "Histoire", inutilityLevel: 30 },
            { text: "Les pommes flottent parce qu'elles sont composées à 25% d'air.", category: "Sciences", inutilityLevel: 67 },
            { text: "Les dauphins dorment avec un œil ouvert.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les mouches domestiques vivent en moyenne 15 à 30 jours.", category: "Animaux", inutilityLevel: 45 },
            { text: "Les serpents peuvent entendre à travers leurs mâchoires.", category: "Animaux", inutilityLevel: 81 },
            { text: "La plupart des gens ont peur des araignées, mais les araignées ont plus peur des humains.", category: "Animaux", inutilityLevel: 76 },
            { text: "Les lamas sont très sélectifs et ne crachent que sur les personnes qu'ils n'aiment pas.", category: "Animaux", inutilityLevel: 89 },
            { text: "Le son d'un pet de baleine bleue est plus fort qu'un moteur d'avion.", category: "Animaux", inutilityLevel: 93 },
            { text: "Les paresseux sont si lents que des algues poussent sur leur fourrure.", category: "Animaux", inutilityLevel: 91 },
            { text: "Les kangourous ne peuvent pas sauter en arrière.", category: "Animaux", inutilityLevel: 72 },
            { text: "Les fourmis peuvent soulever 50 fois leur propre poids.", category: "Animaux", inutilityLevel: 69 },
            { text: "Les araignées sont capables de voler en utilisant de l'électricité statique.", category: "Sciences", inutilityLevel: 87 },
            { text: "Le cerveau humain est plus actif la nuit que le jour.", category: "Corps Humain", inutilityLevel: 55 },
            { text: "Un éclair est cinq fois plus chaud que la surface du soleil.", category: "Sciences", inutilityLevel: 78 },
            { text: "Les poulpes ont trois cœurs.", category: "Animaux", inutilityLevel: 84 },
            { text: "Les chèvres ont des pupilles rectangulaires.", category: "Animaux", inutilityLevel: 86 },
            { text: "La durée de vie moyenne d'un globule rouge est de 120 jours.", category: "Corps Humain", inutilityLevel: 40 },
            { text: "Il faut environ 8 minutes et 20 secondes à la lumière du soleil pour atteindre la Terre.", category: "Sciences", inutilityLevel: 63 },
            { text: "Le cri d'un canard ne produit pas d'écho.", category: "Animaux", inutilityLevel: 94 },
            { text: "Les chats ne peuvent pas goûter le sucre.", category: "Animaux", inutilityLevel: 70 },
            { text: "Les hiboux sont les seuls oiseaux qui peuvent voir la couleur bleue.", category: "Animaux", inutilityLevel: 75 },
            { text: "Les cochons d'Inde ne sont pas des cochons et ne viennent pas de Guinée.", category: "Animaux", inutilityLevel: 80 },
            { text: "Les autruches ont des yeux plus grands que leur cerveau.", category: "Animaux", inutilityLevel: 90 },
            { text: "Les ours polaires ont la peau noire sous leur fourrure blanche.", category: "Animaux", inutilityLevel: 65 },
            { text: "La plus grande pizza jamais faite mesurait 37,8 mètres de diamètre.", category: "Culture Pop", inutilityLevel: 88 },
            { text: "Les humains partagent 50% de leur ADN avec une banane.", category: "Sciences", inutilityLevel: 92 },
            { text: "Le mot 'nerd' a été inventé par Dr. Seuss.", category: "Culture Pop", inutilityLevel: 77 },
            { text: "Les abeilles peuvent être entraînées à détecter des explosifs.", category: "Animaux", inutilityLevel: 81 },
            { text: "Le cri du dodo était si fort qu'il pouvait être entendu à des kilomètres.", category: "Histoire", inutilityLevel: 85 },
            { text: "Les vombats produisent des excréments cubiques.", category: "Animaux", inutilityLevel: 98 },
            { text: "Les papillons goûtent avec leurs pieds.", category: "Animaux", inutilityLevel: 73 },
            { text: "Les girafes se nettoient les oreilles avec leur langue de 50 cm.", category: "Animaux", inutilityLevel: 89 },
            { text: "Les étoiles de mer peuvent régénérer des membres perdus.", category: "Animaux", inutilityLevel: 60 },
            { text: "Le nom 'Google' est une faute d'orthographe de 'googol', un nombre qui est 1 suivi de 100 zéros.", category: "Culture Pop", inutilityLevel: 75 },
            { text: "Les caméléons peuvent bouger leurs yeux dans des directions différentes indépendamment.", category: "Animaux", inutilityLevel: 79 },
            { text: "Le plus ancien chewing-gum a 9 000 ans.", category: "Histoire", inutilityLevel: 68 },
            { text: "Les moustiques sont attirés par les personnes qui viennent de manger des bananes.", category: "Animaux", inutilityLevel: 82 },
            { text: "Les poulets peuvent reconnaître plus de 100 visages différents.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les lapins et les perroquets peuvent voir derrière eux sans bouger la tête.", category: "Animaux", inutilityLevel: 74 },
            { text: "Le plus long mot en français sans 'e' est 'constitutionnellement'.", category: "Culture Pop", inutilityLevel: 87 },
            { text: "Une personne moyenne passera six mois de sa vie à attendre aux feux rouges.", category: "Corps Humain", inutilityLevel: 90 },
            { text: "Les cafards peuvent vivre plusieurs semaines sans tête.", category: "Animaux", inutilityLevel: 95 },
            { text: "Les pingouins ont une glande spéciale qui leur permet de boire de l'eau salée.", category: "Animaux", inutilityLevel: 65 },
            { text: "Les grains de maïs éclatés sont appelés 'popcorn' car ils éclatent en chauffant.", category: "Culture Pop", inutilityLevel: 40 },
            { text: "Les serpents peuvent survivre pendant deux ans sans manger.", category: "Animaux", inutilityLevel: 83 },
            { text: "La couleur orange a été nommée d'après le fruit, pas l'inverse.", category: "Culture Pop", inutilityLevel: 70 },
            { text: "Les escargots ont des milliers de dents microscopiques sur leur langue.", category: "Animaux", inutilityLevel: 78 },
            { text: "Les poissons rouges ont une mémoire de trois secondes.", category: "Animaux", inutilityLevel: 50 },
            { text: "Le saviez-vous ? Le mot 'trivia' vient du latin 'trivium', signifiant 'trois chemins', où des informations banales étaient partagées.", category: "Histoire", inutilityLevel: 80 },
            { text: "Les chats passent 70% de leur vie à dormir.", category: "Animaux", inutilityLevel: 60 },
            { text: "Les hippopotames peuvent courir plus vite que les humains.", category: "Animaux", inutilityLevel: 67 },
            { text: "Les chauves-souris sont les seuls mammifères capables de voler.", category: "Animaux", inutilityLevel: 55 },
            { text: "Les pommes de terre sont composées à 80% d'eau.", category: "Sciences", inutilityLevel: 45 },
            { text: "Les lapins mangent leurs propres excréments pour obtenir plus de nutriments.", category: "Animaux", inutilityLevel: 91 },
            { text: "Le bruit que vous entendez lorsque vous craquez vos doigts est le gaz qui s'échappe de vos articulations.", category: "Corps Humain", inutilityLevel: 72 },
            { text: "Les yeux d'une autruche sont plus grands que son cerveau.", category: "Animaux", inutilityLevel: 85 },
            { text: "Les chats ne peuvent pas voir leur nez.", category: "Animaux", inutilityLevel: 79 },
            { text: "Les abeilles meurent après avoir piqué.", category: "Animaux", inutilityLevel: 68 },
            { text: "Les ours peuvent courir aussi vite que les chevaux.", category: "Animaux", inutilityLevel: 74 },
            { text: "Le cerveau d'Einstein a été volé après sa mort.", category: "Histoire", inutilityLevel: 93 },
            { text: "Les toilettes ont été inventées avant le papier toilette.", category: "Histoire", inutilityLevel: 88 },
            { text: "Les zèbres sont noirs avec des rayures blanches, pas l'inverse.", category: "Animaux", inutilityLevel: 82 },
            { text: "Les escargots peuvent dormir pendant trois ans.", category: "Animaux", inutilityLevel: 90 },
            { text: "Le cœur d'une baleine bleue bat seulement 6 fois par minute.", category: "Animaux", inutilityLevel: 77 },
            { text: "Les humains sont les seuls animaux à pleurer des larmes émotionnelles.", category: "Corps Humain", inutilityLevel: 65 },
            { text: "La seule nourriture qui ne se gâte jamais est le miel.", category: "Sciences", inutilityLevel: 60 },
            { text: "Les étoiles de mer n'ont pas de sang.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les phoques ont des moustaches aussi sensibles que les empreintes digitales humaines.", category: "Animaux", inutilityLevel: 84 },
            { text: "Le bruit d'un éclair est le son de l'air qui se dilate rapidement.", category: "Sciences", inutilityLevel: 58 },
            { text: "Les caméléons changent de couleur en fonction de leur humeur, pas seulement de leur environnement.", category: "Animaux", inutilityLevel: 86 },
            { text: "Les fourmis sont capables de créer des ponts vivants pour traverser des obstacles.", category: "Animaux", inutilityLevel: 75 },
            { text: "Les poissons rouges peuvent vivre jusqu'à 40 ans s'ils sont bien soignés.", category: "Animaux", inutilityLevel: 62 },
            { text: "Les pandas géants mangent environ 12 heures par jour.", category: "Animaux", inutilityLevel: 55 },
            { text: "Le plus grand iceberg jamais enregistré était plus grand que la Belgique.", category: "Sciences", inutilityLevel: 89 },
            { text: "Les bébés naissent avec 300 os, mais les adultes n'en ont que 206.", category: "Corps Humain", inutilityLevel: 70 },
            { text: "Les chevaux ne peuvent pas vomir.", category: "Animaux", inutilityLevel: 73 },
            { text: "Les serpents ont des paupières transparentes qui restent fermées.", category: "Animaux", inutilityLevel: 76 },
            { text: "Les requins n'ont pas d'os, leur squelette est fait de cartilage.", category: "Animaux", inutilityLevel: 69 },
            { text: "Les chauves-souris utilisent l'écholocation pour naviguer dans l'obscurité.", category: "Animaux", inutilityLevel: 64 },
            { text: "Les humains ont plus de bactéries dans leur bouche que d'habitants sur Terre.", category: "Corps Humain", inutilityLevel: 94 },
            { text: "Les chèvres ont un accent.", category: "Animaux", inutilityLevel: 96 },
            { text: "Les chats peuvent faire plus de 100 sons différents, tandis que les chiens n'en font qu'environ 10.", category: "Animaux", inutilityLevel: 55 },
            { text: "Le cri d'un paon est si fort qu'il peut endommager l'ouïe humaine à proximité.", category: "Animaux", inutilityLevel: 77 },
            { text: "Les otaries peuvent danser en rythme avec la musique.", category: "Animaux", inutilityLevel: 80 },
            { text: "Le plus long mot en anglais sans voyelle est 'rhythms'.", category: "Culture Pop", inutilityLevel: 85 },
            { text: "Le miel est le seul aliment qui ne se gâte jamais.", category: "Sciences", inutilityLevel: 65 },
            { text: "Les vaches ont un meilleur ami et deviennent stressées si elles en sont séparées.", category: "Animaux", inutilityLevel: 73 },
            { text: "Les manchots n'ont pas de genoux.", category: "Animaux", inutilityLevel: 68 },
            { text: "Le point sur le 'i' s'appelle un 'tittle'.", category: "Culture Pop", inutilityLevel: 88 },
            { text: "Les requins sont les seuls poissons qui peuvent cligner des yeux.", category: "Animaux", inutilityLevel: 79 },
            { text: "Il y a plus de six fois plus de poulets que d'humains dans le monde.", category: "Animaux", inutilityLevel: 60 },
            { text: "Les rats peuvent rire quand ils sont chatouillés.", category: "Animaux", inutilityLevel: 82 },
            { text: "Le sang des limules est bleu.", category: "Sciences", inutilityLevel: 74 },
            { text: "Les chaussettes ont été inventées pour protéger les pieds.", category: "Histoire", inutilityLevel: 30 },
            { text: "Les pommes flottent parce qu'elles sont composées à 25% d'air.", category: "Sciences", inutilityLevel: 67 },
            { text: "Les dauphins dorment avec un œil ouvert.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les mouches domestiques vivent en moyenne 15 à 30 jours.", category: "Animaux", inutilityLevel: 45 },
            { text: "Les serpents peuvent entendre à travers leurs mâchoires.", category: "Animaux", inutilityLevel: 81 },
            { text: "La plupart des gens ont peur des araignées, mais les araignées ont plus peur des humains.", category: "Animaux", inutilityLevel: 76 },
            { text: "Les lamas sont très sélectifs et ne crachent que sur les personnes qu'ils n'aiment pas.", category: "Animaux", inutilityLevel: 89 },
            { text: "Le son d'un pet de baleine bleue est plus fort qu'un moteur d'avion.", category: "Animaux", inutilityLevel: 93 },
            { text: "Les paresseux sont si lents que des algues poussent sur leur fourrure.", category: "Animaux", inutilityLevel: 91 },
            { text: "Les kangourous ne peuvent pas sauter en arrière.", category: "Animaux", inutilityLevel: 72 },
            { text: "Les fourmis peuvent soulever 50 fois leur propre poids.", category: "Animaux", inutilityLevel: 69 },
            { text: "Les araignées sont capables de voler en utilisant de l'électricité statique.", category: "Sciences", inutilityLevel: 87 },
            { text: "Le cerveau humain est plus actif la nuit que le jour.", category: "Corps Humain", inutilityLevel: 55 },
            { text: "Un éclair est cinq fois plus chaud que la surface du soleil.", category: "Sciences", inutilityLevel: 78 },
            { text: "Les poulpes ont trois cœurs.", category: "Animaux", inutilityLevel: 84 },
            { text: "Les chèvres ont des pupilles rectangulaires.", category: "Animaux", inutilityLevel: 86 },
            { text: "La durée de vie moyenne d'un globule rouge est de 120 jours.", category: "Corps Humain", inutilityLevel: 40 },
            { text: "Il faut environ 8 minutes et 20 secondes à la lumière du soleil pour atteindre la Terre.", category: "Sciences", inutilityLevel: 63 },
            { text: "Le cri d'un canard ne produit pas d'écho.", category: "Animaux", inutilityLevel: 94 },
            { text: "Les chats ne peuvent pas goûter le sucre.", category: "Animaux", inutilityLevel: 70 },
            { text: "Les hiboux sont les seuls oiseaux qui peuvent voir la couleur bleue.", category: "Animaux", inutilityLevel: 75 },
            { text: "Les cochons d'Inde ne sont pas des cochons et ne viennent pas de Guinée.", category: "Animaux", inutilityLevel: 80 },
            { text: "Les autruches ont des yeux plus grands que leur cerveau.", category: "Animaux", inutilityLevel: 90 },
            { text: "Les ours polaires ont la peau noire sous leur fourrure blanche.", category: "Animaux", inutilityLevel: 65 },
            { text: "La plus grande pizza jamais faite mesurait 37,8 mètres de diamètre.", category: "Culture Pop", inutilityLevel: 88 },
            { text: "Les humains partagent 50% de leur ADN avec une banane.", category: "Sciences", inutilityLevel: 92 },
            { text: "Le mot 'nerd' a été inventé par Dr. Seuss.", category: "Culture Pop", inutilityLevel: 77 },
            { text: "Les abeilles peuvent être entraînées à détecter des explosifs.", category: "Animaux", inutilityLevel: 81 },
            { text: "Le cri du dodo était si fort qu'il pouvait être entendu à des kilomètres.", category: "Histoire", inutilityLevel: 85 },
            { text: "Les vombats produisent des excréments cubiques.", category: "Animaux", inutilityLevel: 98 },
            { text: "Les papillons goûtent avec leurs pieds.", category: "Animaux", inutilityLevel: 73 },
            { text: "Les girafes se nettoient les oreilles avec leur langue de 50 cm.", category: "Animaux", inutilityLevel: 89 },
            { text: "Les étoiles de mer peuvent régénérer des membres perdus.", category: "Animaux", inutilityLevel: 60 },
            { text: "Le nom 'Google' est une faute d'orthographe de 'googol', un nombre qui est 1 suivi de 100 zéros.", category: "Culture Pop", inutilityLevel: 75 },
            { text: "Les caméléons peuvent bouger leurs yeux dans des directions différentes indépendamment.", category: "Animaux", inutilityLevel: 79 },
            { text: "Le plus ancien chewing-gum a 9 000 ans.", category: "Histoire", inutilityLevel: 68 },
            { text: "Les moustiques sont attirés par les personnes qui viennent de manger des bananes.", category: "Animaux", inutilityLevel: 82 },
            { text: "Les poulets peuvent reconnaître plus de 100 visages différents.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les lapins et les perroquets peuvent voir derrière eux sans bouger la tête.", category: "Animaux", inutilityLevel: 74 },
            { text: "Le plus long mot en français sans 'e' est 'constitutionnellement'.", category: "Culture Pop", inutilityLevel: 87 },
            { text: "Une personne moyenne passera six mois de sa vie à attendre aux feux rouges.", category: "Corps Humain", inutilityLevel: 90 },
            { text: "Les cafards peuvent vivre plusieurs semaines sans tête.", category: "Animaux", inutilityLevel: 95 },
            { text: "Les pingouins ont une glande spéciale qui leur permet de boire de l'eau salée.", category: "Animaux", inutilityLevel: 65 },
            { text: "Les grains de maïs éclatés sont appelés 'popcorn' car ils éclatent en chauffant.", category: "Culture Pop", inutilityLevel: 40 },
            { text: "Les serpents peuvent survivre pendant deux ans sans manger.", category: "Animaux", inutilityLevel: 83 },
            { text: "La couleur orange a été nommée d'après le fruit, pas l'inverse.", category: "Culture Pop", inutilityLevel: 70 },
            { text: "Les escargots ont des milliers de dents microscopiques sur leur langue.", category: "Animaux", inutilityLevel: 78 },
            { text: "Les poissons rouges ont une mémoire de trois secondes.", category: "Animaux", inutilityLevel: 50 },
            { text: "Le saviez-vous ? Le mot 'trivia' vient du latin 'trivium', signifiant 'trois chemins', où des informations banales étaient partagées.", category: "Histoire", inutilityLevel: 80 },
            { text: "Les chats passent 70% de leur vie à dormir.", category: "Animaux", inutilityLevel: 60 },
            { text: "Les hippopotames peuvent courir plus vite que les humains.", category: "Animaux", inutilityLevel: 67 },
            { text: "Les chauves-souris sont les seuls mammifères capables de voler.", category: "Animaux", inutilityLevel: 55 },
            { text: "Les pommes de terre sont composées à 80% d'eau.", category: "Sciences", inutilityLevel: 45 },
            { text: "Les lapins mangent leurs propres excréments pour obtenir plus de nutriments.", category: "Animaux", inutilityLevel: 91 },
            { text: "Le bruit que vous entendez lorsque vous craquez vos doigts est le gaz qui s'échappe de vos articulations.", category: "Corps Humain", inutilityLevel: 72 },
            { text: "Les yeux d'une autruche sont plus grands que son cerveau.", category: "Animaux", inutilityLevel: 85 },
            { text: "Les chats ne peuvent pas voir leur nez.", category: "Animaux", inutilityLevel: 79 },
            { text: "Les abeilles meurent après avoir piqué.", category: "Animaux", inutilityLevel: 68 },
            { text: "Les ours peuvent courir aussi vite que les chevaux.", category: "Animaux", inutilityLevel: 74 },
            { text: "Le cerveau d'Einstein a été volé après sa mort.", category: "Histoire", inutilityLevel: 93 },
            { text: "Les toilettes ont été inventées avant le papier toilette.", category: "Histoire", inutilityLevel: 88 },
            { text: "Les zèbres sont noirs avec des rayures blanches, pas l'inverse.", category: "Animaux", inutilityLevel: 82 },
            { text: "Les escargots peuvent dormir pendant trois ans.", category: "Animaux", inutilityLevel: 90 },
            { text: "Le cœur d'une baleine bleue bat seulement 6 fois par minute.", category: "Animaux", inutilityLevel: 77 },
            { text: "Les humains sont les seuls animaux à pleurer des larmes émotionnelles.", category: "Corps Humain", inutilityLevel: 65 },
            { text: "La seule nourriture qui ne se gâte jamais est le miel.", category: "Sciences", inutilityLevel: 60 },
            { text: "Les étoiles de mer n'ont pas de sang.", category: "Animaux", inutilityLevel: 71 },
            { text: "Les phoques ont des moustaches aussi sensibles que les empreintes digitales humaines.", category: "Animaux", inutilityLevel: 84 },
            { text: "Le bruit d'un éclair est le son de l'air qui se dilate rapidement.", category: "Sciences", inutilityLevel: 58 },
            { text: "Les caméléons changent de couleur en fonction de leur humeur, pas seulement de leur environnement.", category: "Animaux", inutilityLevel: 86 },
            { text: "Les fourmis sont capables de créer des ponts vivants pour traverser des obstacles.", category: "Animaux", inutilityLevel: 75 },
            { text: "Les poissons rouges peuvent vivre jusqu'à 40 ans s'ils sont bien soignés.", category: "Animaux", inutilityLevel: 62 },
            { text: "Les pandas géants mangent environ 12 heures par jour.", category: "Animaux", inutilityLevel: 55 },
            { text: "Le plus grand iceberg jamais enregistré était plus grand que la Belgique.", category: "Sciences", inutilityLevel: 89 },
            { text: "Les bébés naissent avec 300 os, mais les adultes n'en ont que 206.", category: "Corps Humain", inutilityLevel: 70 },
            { text: "Les chevaux ne peuvent pas vomir.", category: "Animaux", inutilityLevel: 73 },
            { text: "Les serpents ont des paupières transparentes qui restent fermées.", category: "Animaux", inutilityLevel: 76 },
            { text: "Les requins n'ont pas d'os, leur squelette est fait de cartilage.", category: "Animaux", inutilityLevel: 69 },
            { text: "Les chauves-souris utilisent l'écholocation pour naviguer dans l'obscurité.", category: "Animaux", inutilityLevel: 64 },
            { text: "Les humains ont plus de bactéries dans leur bouche que d'habitants sur Terre.", category: "Corps Humain", inutilityLevel: 94 },
            { text: "Les chèvres ont un accent.", category: "Animaux", inutilityLevel: 96 }
        ];

        // Éléments du DOM
        const newFactBtn = document.getElementById('new-fact-btn');
        const currentFactDisplay = document.getElementById('current-fact');
        const factCategoryDisplay = document.getElementById('fact-category');
        const inutilityProgress = document.getElementById('inutility-progress');
        const inutilityText = document.getElementById('inutility-text');
        const voteYesBtn = document.getElementById('vote-yes-btn');
        const voteNoBtn = document.getElementById('vote-no-btn');
        const usefulCountSpan = document.getElementById('useful-count');
        const uselessCountSpan = document.getElementById('useless-count');
        const shareFactBtn = document.getElementById('share-fact-btn');
        const submitFactTextarea = document.getElementById('submit-fact-text');
        const submitFactBtn = document.getElementById('submit-fact-btn');
        const messageBox = document.getElementById('message-box');

        // Variables pour les statistiques de vote (non persistantes)
        let usefulVotes = 0;
        let uselessVotes = 0;
        let currentFact = null; // Pour stocker le fait actuellement affiché

        // Fonction pour afficher un message temporaire
        function showMessage(message) {
            messageBox.textContent = message;
            messageBox.classList.add('show');
            setTimeout(() => {
                messageBox.classList.remove('show');
            }, 2000);
        }

        // Fonction pour obtenir un fait inutile aléatoire
        function getRandomFact() {
            const randomIndex = Math.floor(Math.random() * uselessFacts.length);
            return uselessFacts[randomIndex];
        }

        // Fonction pour mettre à jour l'affichage du fait
        function updateFactDisplay() {
            currentFact = getRandomFact();
            currentFactDisplay.textContent = currentFact.text;
            factCategoryDisplay.textContent = currentFact.category;

            // Mettre à jour la barre de progression
            const inutilityPercentage = currentFact.inutilityLevel;
            inutilityProgress.style.width = `${inutilityPercentage}%`;
            inutilityText.textContent = `${inutilityPercentage}% d'inutilité`;

            // Réinitialiser les votes pour le nouveau fait (pour cette démo simple)
            usefulVotes = 0;
            uselessVotes = 0;
            updateVoteCounts();
        }

        // Fonction pour mettre à jour les compteurs de vote
        function updateVoteCounts() {
            usefulCountSpan.textContent = usefulVotes;
            uselessCountSpan.textContent = uselessVotes;
        }

        // Gestionnaire d'événements pour le bouton "Donne-moi un fait inutile"
        newFactBtn.addEventListener('click', updateFactDisplay);

        // Gestionnaires d'événements pour les boutons de vote
        voteYesBtn.addEventListener('click', () => {
            usefulVotes++;
            updateVoteCounts();
            showMessage("Merci pour votre vote !");
        });

        voteNoBtn.addEventListener('click', () => {
            uselessVotes++;
            updateVoteCounts();
            showMessage("Excellent ! L'inutilité est notre force.");
        });

        // Gestionnaire d'événements pour le bouton de partage
        shareFactBtn.addEventListener('click', () => {
            if (currentFact) {
                // Utilisation de document.execCommand pour la compatibilité dans les iframes
                const textArea = document.createElement("textarea");
                textArea.value = `Inutilepedia : Saviez-vous que "${currentFact.text}" ? #FaitInutile`;
                document.body.appendChild(textArea);
                textArea.select();
                try {
                    document.execCommand('copy');
                    showMessage("Fait copié ! Partagez l'inutilité !");
                } catch (err) {
                    console.error('Impossible de copier le texte', err);
                    showMessage("Erreur lors de la copie du fait.");
                }
                document.body.removeChild(textArea);
            } else {
                showMessage("Pas de fait à copier pour le moment.");
            }
        });

        // Gestionnaire d'événements pour la soumission de fait
        submitFactBtn.addEventListener('click', () => {
            const submittedFact = submitFactTextarea.value.trim();
            if (submittedFact) {
                // Ici, vous enverriez normalement le fait à un backend pour modération
                // Pour cette démo, on simule juste l'envoi
                console.log("Fait soumis :", submittedFact);
                submitFactTextarea.value = ''; // Vider le champ
                showMessage("Votre fait a été soumis pour modération. Merci !");
            } else {
                showMessage("Veuillez taper un fait avant de soumettre.");
            }
        });

        // Initialiser l'affichage au chargement de la page
        window.onload = updateFactDisplay;
    </script>
</body>
</html>
