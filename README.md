# TP1-Analyse-spectrale-d-un-signal
## Transformée de Fourier discrète


<a name="retour"></a>
## Sommaire :
1. [ Objectifs. ](#objectif)
1. [ Représentation temporelle et fréquentielle. ](#part1)
2. [ Analyse fréquentielle du chant du rorqual bleu. ](#part2)

<a name="objectif"></a>
### **1. Objectifs:**
 ~Representation de signaux et applications de la transformee de Fourier discrete(TFD) sous Matlab
 
 ~Evaluation de l'interet du passage du domaine temporel au domaine frequentiel dans l'analyse et l'interpretation des signaux physiques reels
 
#### Commentaires :
Il est à remarquer que ce TP traite en principe des signaux continus.Or, l'utilisation de Matlab suppose l'échantillonnage du signal. Il faudra donc être vigilant par rapport aux différences de traitement entre le temps continu et le temps discret.
 
#### Tracé des figures :
toutes les figures devront être tracées avec les axes et les légendes des axes appropriés.

#### Travail demandé :
un script Matlab commenté contenant le travail réalisé et des commentaires sur ce que vous avez compris et pas compris, ou sur ce qui vous a semblé intéressant ou pas, bref tout commentaire pertinent sur le TP

$~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$ [ (Revenir au sommaire) ](#retour)


<a name="part1"></a>
### **1. Représentation temporelle et fréquentielle:**
Considérons un signal périodique x(t) constitué d’une somme de trois sinusoïdes de fréquences 440Hz, 550Hz, 2500Hz.
#### **𝐱(𝐭) = 𝟏. 𝟐𝐜𝐨𝐬(𝟐𝐩𝐢𝟒𝟒𝟎𝐭 + 𝟏. 𝟐) + 𝟑𝐜𝐨𝐬(𝟐𝐩𝐢𝟓𝟓𝟎𝐭) + 𝟎. 𝟔𝐜𝐨𝐬(𝟐𝐩𝐢𝟐𝟓𝟎𝟎𝐭)**

####  **1- Tracer le signal x(t). Fréquence d’échantillonnage : fe = 10000Hz, Intervalle : Nombre d’échantillons : N = 5000.**

```matlab

fe = 1e4;  % frequence d echantillonnage
te = 1/fe; % pas d echantillonnage
N = 10000; % nombre d echantillons (points)
t = (0:N-1)*te; % intervalle de temps 
x = 1.2*cos(2*pi*440*t+1.2)+3*cos(2*pi*550*t)+0.6*cos(2*pi*2500*t);
plot(t,x,'.');
title('signal x(t) :');

```
<img width="809" alt="1" src="https://user-images.githubusercontent.com/93081417/210829647-0608e8e4-8823-48e7-9fa3-91c336cc3e9c.png">

Pour approximer la TF continue d’un signal x(t), représenté suivant un pas Te, on utilise les deux commandes : fft et fftshift.

• On remarquera que la TF est une fonction complexe et que la fonction ainsi obtenue décrit la TF de x(t) entre –1/(2Te) et 1/(2Te) par pas de 1/(nTe) où n est le nombre de points constituant le signal x(t).

• La commande fft codant les fréquences positives sur les n/2 premières valeurs du signal et les valeurs négatives entre n/2+1 et n, la commande fftshift permet de les inverser.

####  **2- Calculer la TFD du signal x(t) en utilisant la commande fft, puis tracer son spectre en amplitude après avoir créé le vecteur f qui correspond à l'échantillonnage du signal dans l'espace fréquentiel. Utiliser la commande abs pour afficher le spectre d’amplitude.**



```matlab

 f =(0:N-1)*(fe/N); %frequence du spectre
 y = fft(x); % y: spectre , fft(x) : transformee de fourier
 plot(f,abs(y));
 title('le spectre Amplitude avec fft');

```
<img width="860" alt="2" src="https://user-images.githubusercontent.com/93081417/210832054-cf9c3969-7a7a-4727-9248-2b80c06a43df.png">


####  **3. Pour mieux visualiser le contenu fréquentiel du signal, utiliser la fonction fftshift, que effectue un décalage circulaire centré sur zéro du spectre en amplitude obtenu par la commande fft**

```matlab
 fshift = (-N/2:N/2-1)*(fe/N);
 plot(fshift,fftshift(abs(y)));
title('spectre du  x(t) apres fftshift():');

```

<img width="826" alt="3" src="https://user-images.githubusercontent.com/93081417/210833376-270192bc-6331-483e-abe9-d91ee36f210d.png">
