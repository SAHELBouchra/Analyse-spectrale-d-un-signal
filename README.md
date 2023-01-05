# TP1-Analyse-spectrale-d-un-signal
## Transformée de Fourier discrète



***
<a name="retour"></a>
## Sommaire :
1. [ Représentation temporelle et fréquentielle. ](#part1)
2. [ Analyse fréquentielle du chant du rorqual bleu. ](#part2)
***
<a name="part1"></a>
### **1. Représentation temporelle et fréquentielle:**
***
Considérons un signal périodique x(t) constitué d’une somme de trois sinusoïdes de 
fréquences 440Hz, 550Hz, 2500Hz.
#### $~~~~~~~~~~~~~~~~~~~~$ **𝐱(𝐭) = 𝟏. 𝟐𝐜𝐨𝐬(𝟐𝐩𝐢𝟒𝟒𝟎𝐭 + 𝟏. 𝟐) + 𝟑𝐜𝐨𝐬(𝟐𝐩𝐢𝟓𝟓𝟎𝐭) + 𝟎. 𝟔𝐜𝐨𝐬(𝟐𝐩𝐢𝟐𝟓𝟎𝟎𝐭)** 
#### $~~~~~~$ **1- Tracer le signal x(t). Fréquence d’échantillonnage : fe = 10000Hz, Intervalle : Nombre d’échantillons : N = 5000.** 
***
```matlab
% qst 1

fe = 1e4;  % fe: frequence d echantillonnage
te = 1/fe; % te: pas d echantillonnage
N = 10000; % N: nombre d echantillons (points)
t = (0:N-1)*te; % intervalle de temps x(0),x(te),x(2te),...,x((N-1)te).
x = 1.2*cos(2*pi*440*t+1.2)+3*cos(2*pi*550*t)+0.6*cos(2*pi*2500*t);
plot(t,x,'.');
title('x(t) :');

```
![1](https://user-images.githubusercontent.com/106840796/210169816-41814ce5-e05f-4a96-a800-8bcf39693f37.PNG)
***
 ### **Explication :**
 ###### Matlab ne représente pas les signaux continus , par contre , il représente seulement les signaux discrets , en utilisant la notion d échantillonnage . dans notre exercice , la période d échantillonnage te=1/fe=1/10000 alors notre intervalle de temps sera t=(0:N-1)*te , donc, matlab va tracer les 5000 points suivants : x(0) , x(te) , x(2te) , x(3te) , ... , x(4999te)
***
#### $~~~~~~$ **2- Calculer la TFD du signal x(t) en utilisant la commande fft, puis tracer son spectre en amplitude après avoir créé le             vecteur f qui correspond à l'échantillonnage du signal dans l'espace fréquentiel. Utiliser la commande abs pour afficher le spectre          d’amplitude.** 
***
```matlab
% qst 2

f =(0:N-1)*(fe/N); % f: frequence du spectre
y = fft(x);     % y: spectre , fft(x) : transformee de fourier
plot(f,abs(y)); % abs(y) : spectre d amplitude
title('spectre du  x(t) :');

```
![2](https://user-images.githubusercontent.com/106840796/210169818-e6f3ad1c-8e53-4253-9c02-0d89edb6bbcc.PNG)
***
 ### **Explication :**
 ###### Matlab utilise la transformée de Fourier discrète pour passer du domaine temporel au domaine fréquentiel , en gardant le même nombre d échantillons N=5000 . On remarque que la TF est une fonction complexe et que la fonction ainsi obtenue décrit la TF de x(t) entre –1/(2Te) et 1/(2Te) par pas de 1/(nTe) où n est le nombre de points constituant le signal x(t).La commande fft codant les fréquences positives sur les n/2 premières valeurs du signal et les valeurs négatives entre n/2+1 et n .

***
#### $~~~~~~$ ** 3- Pour mieux visualiser le contenu fréquentiel du signal, utiliser la fonction fftshift, qui effectue un décalage circulaire centré sur zéro du spectre en amplitude obtenu par la commande fft.** 
***
```matlab
% qst 3

fshift = (-N/2:N/2-1)*(fe/N)
plot(fshift,fftshift(abs(y)))
title('spectre du  x(t) apres fftshift():');


```
![3](https://user-images.githubusercontent.com/106840796/210169545-116a1b53-49f5-48da-846e-4cc7a3702c48.PNG)
***
 ### **Explication :**
 ###### fftshift est une fonction qui effectue un décalage circulaire centré sur zéro du spectre en amplitude obtenu par lacommande fft. .

***
$~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$ [ (Revenir au sommaire) ](#retour)
***
##### Un bruit correspond à tout phénomène perturbateur gênant la transmission ou l'interprétation d'un signal. Dans les applications scientifiques, les signaux sont souvent corrompus par du bruit aléatoire, modifiant ainsi leurs composantes fréquentielles. La TFD peut traiter le bruit aléatoire et révéler les fréquences qui y correspond.
***
#### $~~~~~~$ **4- Créer un nouveau signal xnoise, en introduisant un bruit blanc gaussien dans le 
signal d’origine x(t), puis visualisez-le. Utiliser la commande randn pour générer ce 
bruit. Il est à noter qu’un bruit blanc est une réalisation d'un processus aléatoire dans 
lequel la densité spectrale de puissance est la même pour toutes les fréquences de 
la bande passante. Ce bruit suit une loi normale de moyenne 0 et d’écart type 1** 
***
```matlab
% qst 4

bruit = 5*randn(size(x));
xnoise= x+bruit;
subplot(1,2,1)
plot(bruit)
title('bruit');
subplot(1,2,2)
plot(fshift,fftshift(abs(fft(xnoise))));
title('spectre du signal bruité');

```
![4](https://user-images.githubusercontent.com/106840796/210172766-ba59b83b-2c55-45ee-833b-b813e4dafd7b.PNG)
***
 ### **Explication :**
 ###### xnoise est un bruit généré par la fonction randn() qui suit la loi gaussienne de moyenne 0 et d’écart type 1. 97% des valeurs générées par la fonction randn() se situent entre -3 (moyenne - 3* ecartType = 0-3*1=-3) et 3 (moyenne + 3* ecartType = 0+3*1=3).
 ###### Ainsi , on remarque l ajout des perturbations de haute frequence dans le spectre du signal bruité , c est le bruit .

***
$~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$ [ (Revenir au sommaire) ](#retour)
***
#### $~~~~~~$ **5 – Utiliser la commande sound pour écouter le signal et puis le signal bruité.** 
***
```matlab
% qst 5

sound(bruit,fe)
sound(xnoise,fe)

```
***
 ### **Explication :**
 ###### sound(xnoise , fe) envoie le signal audio y au haut-parleur à la fréquence d’échantillonnage donnée fe=10000hz , si on a pas inseré la fréquence d’échantillonnage ( sound(xnoise) ), elle sera par défaut egale a 8192 Hz.

***
$~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$ [ (Revenir au sommaire) ](#retour)
***
##### La puissance du signal en fonction de la fréquence (densité spectrale de puissance)est une métrique couramment utilisée en traitement du signal. Elle est définie comme étant le carré du module de la TFD, divisée par le nombre d'échantillons de fréquence.
***
#### $~~~~~~$ **6- Calculez puis tracer le spectre de puissance du signal bruité centré à la fréquence zéro.** 
***
```matlab
% qst 6

f =(0:N-1)*(fe/N); % f: frequence du spectre
y = fft(xnoise);     % y: spectre , fft(x) : transformee de fourier
spectreDePuissance=2.^abs(y)/N;
plot(f,spectreDePuissance); 
title('spectre de puissance du signal bruité :');

```
![6](https://user-images.githubusercontent.com/106840796/210170672-d51df29d-c132-44d4-b481-727677c5c320.PNG)
***
 ### **Explication :**
 ###### La spectre de puissance d'un signal est une mesure de la répartition de l'énergie du signal en fonction de la fréquence. Elle permet de visualiser la composition spectral d'un signal et de déterminer les fréquences présentes dans le signal.

***
$~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$ [ (Revenir au sommaire) ](#retour)
***

#### $~~~~~~$ **7- Augmenter l’intensité de bruit puis afficher le spectre. Interpréter le résultat obtenu.** 
***
```matlab
%qst 7

bruit_haute_intensite = 50*randn(size(x));
xnoise= x+bruit_haute_intensite;
subplot(1,2,1)
plot(bruit_haute_intensite)
title('bruit haute intensite');
subplot(1,2,2)
plot(fshift,fftshift(abs(fft(xnoise))));
title('spectre du signal bruité');

```
![7](https://user-images.githubusercontent.com/106840796/210172657-a91ec71a-780f-40a1-917d-186b483fcfa9.PNG)
***
 ### **Explication :**
 ###### xnoise est un bruit généré par la fonction randn() qui suit la loi gaussienne de moyenne 0 et d’écart type 1. 97% des valeurs générées par la fonction randn() se situent entre -3 (moyenne - 3* ecartType = 0-3*1=-3) et 3 (moyenne + 3* ecartType = 0+3*1=3), alors , on a multiplié par 50 pour augmenter l’intensité de bruit , maintenant , les valeurs générées se situent entre -150 et 150 .

***
$~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$ [ (Revenir au sommaire) ](#retour)
***
<a name="part2"></a>
### **2. Analyse fréquentielle du chant du rorqual bleu**
***
##### Il existe plusieurs signaux dont l’information est encodée dans des sinusoïdes. Les ondes sonores est un bon exemple. Considérons maintenant des données audios collectées à partir de microphones sous - marins au large de la Californie. On cherche à détecter à travers une analyse de Fourier le contenu fréquentiel d’une onde sonore émise pas un rorqual bleu.
***
#### $~~~~~~$ **1- Chargez, depuis le fichier ‘bluewhale.au’, le sous-ensemble de données qui 
correspond au chant du rorqual bleu du Pacifique. En effet, les appels de rorqual bleu 
sont des sons à basse fréquence, ils sont à peine audibles pour les humains. Utiliser 
la commande audioread pour lire le fichier. Le son à récupérer correspond aux indices 
allant de 2.45e4 à 3.10e4.** 
***
```matlab
%qst 1

[x,fe]=audioread("bluewhale.au");
chant = x(2.45e4:3.10e4);


```

***
 ### **Explication :**
 ###### la commande audioread sert a lire le fichier "bluewhale.au" pour construire son signal chant qui correspond aux indices allant de 2.45e4 à 3.10e4 et la frequence fe .

***
$~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$ [ (Revenir au sommaire) ](#retour)
***
#### $~~~~~~$ **2- Ecoutez ce signal en utilisant la commande sound, puis visualisez-le.** 
***
```matlab
%qst 2

sound(chant,fe)
N = length(chant);
te = 1/fe;
t = (0:N-1)*(10*te);
plot(t,chant)
title('signal du chant du rorqual bleu :');

```
![part2 2](https://user-images.githubusercontent.com/106840796/210175084-9a5df19c-38fc-4246-b8c9-d72d3e132560.PNG)
***
 ### **Explication :**
 ###### sound(xnoise , fe) envoie le signal audio y au haut-parleur à sa fréquence d’échantillonnage fe .
 ###### on a compressé le signal en multipliant le temps par 10 en utilisant Matlab, mais cette technique ne fera que réduire la durée du signal, mais ne supprimera pas les données inutiles ou redondantes.

***
$~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$ [ (Revenir au sommaire) ](#retour)
***
##### La TFD peut être utilisée pour identifier les composantes fréquentielles de ce signal audio. Dans certaines applications qui traitent de grandes quantités de données avec fft, il est courant de redimensionner l'entrée de sorte que le nombre d'échantillons soit une puissance de 2. fft remplit automatiquement les données avec des zéros pour augmenter la taille de l'échantillon. Cela peut accélérer considérablement le calcul de la transformation.
***
#### $~~~~~~$ **3- Spécifiez une nouvelle longueur de signal qui sera une puissance de 2, puis tracer 
la densité spectrale de puissance du signal.** 
***
```matlab
%qst 3 

densite_spectrale= abs(fft(chant)).^2/N; 
f = (0:floor(N/2))*(fe/N)/10;
plot(f,densite_spectrale(1:floor(N/2)+1));
title('densité spectrale de puissance du signal:');

```
![qst3](https://user-images.githubusercontent.com/106840796/210177673-64f265b0-d0a3-46a4-99b5-70662aed39ce.PNG)
***
 ### **Explication :**
 ###### ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff

***
$~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$ [ (Revenir au sommaire) ](#retour)
***
#### $~~~~~~$ **4- Déterminer à partir du tracé, la fréquence fondamentale du gémissement de rorqual bleu.** 
***
```matlab
% qst 4

% Recherche de la fréquence fondamentale
[~, index] = max(densite_spectrale);
freqence_fondamentale = f(index)

```

***
 ### **Explication :**
 ###### d apres le tracé , la frequence fondamentale du gémissement est ensuite trouvée en cherchant la fréquence avec la valeur maximale dans le spectre de puissance qui est egale a 50hz .

***
$~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$ [ (Revenir au sommaire) ](#retour)
***

>## **Mariam Aboujenane**
>## **Filiere :** robotique et cobotique .
>## **Encadré par :** Pr. Ammour Alae .

