# Rapport TP1-TS Analyse spectrale d’un signal Transformée de Fourier discrète

## réalisé par SAKASSA RACHID
Pr. AMMOUR Alae

## Objectifs

      • Représentation de signaux et applications de la transformée de Fourier discrète
        (TFD) sous Matlab. 

       • Evaluation de l’intérêt du passage du domaine temporel au domaine fréquentiel
          dans l’analyse et l’interprétation des signaux physiques réels.

## Représentation temporelle et fréquentielle 

     le signal x(t). Pas de temps : Te = 1/50s, Intervalle : 0, 10-Te

 ```Matlab
     clear all
     close all
     clc

     %Partie 1-----------------------------------------------------------------------------------------------------------------------------------

     Te=1/50; 
     fe=50;      
     t=0:Te:10-Te;
     N=length(t);
     figure(1);
     %s un signal périodique x(t) constitué d’une somme de deux sinusoïdes de fréquences 15Hz et 20Hz
     x=sin(2*pi*15*t)+sin(2*pi*20*t);
     subplot(3,2,1);
     %sound(x);
     plot(t,x);
     title('signal periodique');
     
     
```

## la TFD du signal x(t) en utilisant la commande fft
 
```Matlab

       %TFD du signal x(t) en utilisant la commande fft
       F=abs(fft(x));
       f=(0:N-1)*(fe/N);
       subplot(3,2,2);
       plot(f,F);
       title('TFD');
       
 ```


▪ On remarquera que la TF est une fonction complexe et que la fonction ainsi
obtenue décrit la TF de x(t) entre –1/(2Te) et 1/(2Te) par pas de 1/(nTe) où n
est le nombre de points constituant le signal x(t).

▪ La commande fft codant les fréquences positives sur les n/2 premières valeurs
du signal et les valeurs négatives entre n/2+1 et n, la commande fftshift permet
de les inverser.

```Matlab

%Pour mieux visualiser le contenu fréquentiel du signal,on utilise la fonction fftshift
subplot(3,2,3);
fshift=(-N/2:(N/2)-1)*(fe/N);
plot(fshift,fftshift(F));
title('TFD centré à zero');

```

On remarquera que la fonction fftshift, qui effectue un décalage circulaire centré 
sur zéro du spectre en amplitude obtenu par la commande fft.


## L'ajout d'un bruit


*Un bruit correspond à tout phénomène perturbateur gênant la transmission ou
l'interprétation d'un signal. Dans les applications scientifiques, les signaux sont
souvent corrompus par du bruit aléatoire, modifiant ainsi leurs composantes
fréquentielles. La TFD peut traiter le bruit aléatoire et révéler les fréquences qui y
correspond.*

```Matlab

% randn pour générer un xnoice (bruit)
xnoise=x+2*randn(size(t));

subplot(3,2,4);
plot(t,xnoise);

% le spectre de puissance du signal bruité centré à la fréquence zéro
Fnoise=abs(fft(xnoise));
subplot(3,2,5);
plot(fshift,fftshift(Fnoise.^2));

DS=(Fnoise.^2)/N;
subplot(3,2,6);
plot(fshift,fftshift(DS));

```

un nouveau signal xnoise, un bruit blanc gaussien dans le
signal d’origine x(t).la commande randn pour générer ce
bruit. Il est à noter qu’un bruit blanc est une réalisation d'un processus aléatoire dans
lequel la densité spectrale de puissance est la même pour toutes les fréquences de
la bande passante. Ce bruit suit une loi normale de moyenne et variance données.



. Malgré le bruit, ont peut constater qu’il est toujours possible de distinguer les
fréquences du signal en raison des pics de puissance. En plus, on voit que, entre 0Hz
et 15Hz, la densité spectrale de puissance est relativement constante. Ceci est dû au
bruit blanc gaussien.

on remarque toutes les courbes sur cette figure.

![image](https://user-images.githubusercontent.com/85129301/149663806-258628f8-666d-402b-8a1d-916800b3fbe0.png)



## Analyse fréquentielle du chant du rorqual bleu 

-*Il existe plusieurs signaux dont l’information est encodée dans des sinusoïdes. Les
ondes sonores est un bon exemple. Considérons maintenant des données audios
collectées à partir de microphones sous - marins au large de la Californie. On cherche
à détecter à travers une analyse de Fourier le contenu fréquentiel d’une onde sonore
émise pas un rorqual bleu.*

Depuis le fichier ‘bluewhale.au’, le sous-ensemble de données qui
correspond au chant du rorqual bleu du Pacifique. En effet, les appels de rorqual bleu
sont des sons à basse fréquence, ils sont à peine audibles pour les humains. Utiliser
la commande audioread pour lire le fichier. Le son à récupérer correspond aux indices
allant de 2.45e4 à 3.10e4.



```Matlab

whaleFile = fullfile(matlabroot,'examples','matlab','data','bluewhale.au');
[w,ft] = audioread(whaleFile);
chant=w(2.45e4:3.10e4);
subplot(2,1,1);
soundsc(w,fs);

Nchant=length(chant);
t=[0:Nchant-1]*1/ft;
plot(t,chant);
title('courbe du signal bluewhale');


```

La TFD peut être utilisée pour identifier les composantes fréquentielles de ce signal
audio. Dans certaines applications qui traitent de grandes quantités de données avec
fft, il est courant de redimensionner l'entrée de sorte que le nombre d'échantillons soit
une puissance de 2. fft remplit automatiquement les données avec des zéros pour
augmenter la taille de l'échantillon. Cela peut accélérer considérablement le calcul de
la transformation.

```Matlab

%la densité spectrale de puissance du signal
fshift=[-Nchant/2:Nchant/2-1]*(ft/Nchant)/10;
DSP=abs(fft(chant).^2/Nchant);
subplot(2,1,2);
plot(fshift,fftshift(DSP));
title('la densité spectrale de puissance du signal')
```


* On peut Déterminer à partir du tracé, la fréquence fondamentale du gémissement de rorqual
bleu.*

![image](https://user-images.githubusercontent.com/85129301/149664604-28481b50-bfbc-49a2-8cbd-1ade3023551f.png)

#Fin.

#Merci pour votre soutien.



