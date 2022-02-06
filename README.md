# tp3-rapport
# TP3 TS - La corrélation croisée Mesure de similarité entre deux signaux

## Encadré par: Prof AMMOUR ALAA

Objectifs

• Comprendre la notion de la corrélation croisée et évaluer sa capacité dans la
mesure de dépendance deux signaux.

• Evaluer l’efficacité de cette mesure en présence du bruit.

Commentaires : Il est à remarquer que ce TP traite en principe des signaux continus.
Or, l'utilisation de Matlab suppose l'échantillonnage du signal. Il faudra donc être
vigilant par rapport aux différences de traitement entre le temps continu et le temps
discret.


Tracé des figures : toutes les figures devront être tracées avec les axes et les
légendes des axes appropriés.

Travail demandé : un script Matlab commenté contenant le travail réalisé et des
commentaires sur ce que vous avez compris et pas compris, ou sur ce qui vous a
semblé intéressant ou pas, bref tout commentaire pertinent sur le TP.

## Mesure de similarité entre deux signaux

En traitement du signal, la corrélation croisée (aussi appelée covariance croisée) est
la mesure de la similitude entre deux signaux. Afin de mieux appréhender cette notion,
on procédera à l’estimation de la corrélation croisée entre un signal sonore
échantillonné et un fragment de ce signal, en utilisant les outils appropriés fournis par
MATLAB. Le signal sonore qui fera l’objet de cet exercice est celui d’un anneau
tournant sur une table. 

1-On Charge dans l’espace de travail l’enregistrement correspondant en tapant la
commande load('Ring.mat').

```Matlab
 clear all
 close all
 clc
load('Ring.mat');
```

2-On Trace ce signal en fonction du temps.

```Matlab 
Fs=44100;
Ts =1/Fs;
t =0:Ts:(length(y)-1)*Ts ;
 plot(t,y);
  %sound(y,Fs);
  ```

3- En utilisant deux droites en pointillés rouges,on repére le morceau du signal entre
t=7s et t=8s (Commande : xline).

```Matlab 
 xline(7,'--r');
 xline(8,'--r');
 ```
 ![image](https://user-images.githubusercontent.com/85129301/152623806-ecba62f3-033e-4098-819e-e01f407d6413.png)

 
 4-On récupére ce morceau dans une variable nommée « Fragment », , puis on le 
trace en fonction du temps:

```Matlab
 fragment=y(7*Fs:8*Fs);
 %sound(fragment,Fs);
 figure(2)
 plot(fragment)
 ```
 ![image](https://user-images.githubusercontent.com/85129301/152624052-3bddec63-5850-4a31-816b-e55ffa426f95.png)

 
 
 5- ON Calcule eton trace la corrélation croisée du signal complet et du fragment.

```Matlab
  corr=xcorr(y,fragment);
  figure(3)
  plot(corr);
  %Le plus grand pic se produit à la valeur de décalage lorsque les éléments de y et fragment correspondent exactement
  ```
  
![image](https://user-images.githubusercontent.com/85129301/152624469-6c428a91-d120-4f0f-8b60-62046dfa9460.png)

6- Déduisez la valeur du décalage pour lequel la corrélation entre les deux signaux est
maximal, puis utilisez ce décalage pour faire apparaitre le fragment dans le signal. 

```Matlab
 figure(4);
 [m,d]=max(corr);      %find value and index of maximum value of cross-correlation amplitude
 delay=d-max(length(y),length(fragment));   %shift index d, as length(y)=2*N-1; where N is the length of the signals
 plot(y)                                     %Plot signal y
 hold on,plot([delay+1:length(fragment)+delay],fragment,'r');  
```
![image](https://user-images.githubusercontent.com/85129301/152624559-629b6687-79fd-42c0-b5f1-9ca1604786db.png)

## Mesure de similarité entre deux signaux en présence du bruit

Dans cette partie, nous chercherons à évaluer le degré de dépendance de deux
signaux en présence du bruit, en utilisant toujours cette notion de corrélation croisée.

1- On ajoute un bruit blanc gaussien au signal de départ et aussi au fragment, puis
on les trace  en fonction du temps. 

```Matlab 
 ynoise = awgn(y,30)+y;
 fnoise=awgn(fragment,30)+fragment;
 figure(5);
 plot(t,[y ynoise]);
 xline(7,'--r');
 xline(8,'--r');
 figure(6);
 plot(fnoise);
```
*signal de départ bruité:*

![image](https://user-images.githubusercontent.com/85129301/152624669-bdb33a58-a38f-4e5c-a942-fcbc121d6d11.png)

*fragment bruité:*

![image](https://user-images.githubusercontent.com/85129301/152624644-1dff02ff-a306-4eb1-b00c-349764eaed87.png)

2-On applique la fonction xcorr afin de calculer la corrélation croisée entre les deux
signaux bruités, puis on évalue la capacité de cette mesure de détecter le fragment dans
le signal en présence du bruit.Puis on trace toutes les figure:

```Matlab 
corr1=xcorr(ynoise,fnoise);
  figure(7)
  plot(corr1);%Le plus grand pic se produit à la valeur de décalage lorsque les éléments de x et y correspondent exactement
  figure(8);
  [m,d]=max(corr1);      %find value and index of maximum value of cross-correlation amplitude
  delay=d-max(length(ynoise),length(fnoise));   %shift index d, as length(X1)=2*N-1; where N is the length of the signals
  plot(ynoise);
  hold on,plot([delay+1:length(fnoise)+delay],fnoise,'r');
```
voici la corrélation:

![image](https://user-images.githubusercontent.com/85129301/152624842-953d4f84-6ec4-4049-a4ac-aa8576c02025.png)

et voici le signal détecté sur le signal de départ:

![image](https://user-images.githubusercontent.com/85129301/152624820-826829a3-8f17-461f-8ad6-cbdec1fa7f42.png)


On Conclure que Le résultat de xcorr peut être interprété comme une estimation de la corrélation entre deux séquences aléatoires ou comme la corrélation déterministe entre deux signaux déterministes.

FIN.

realisée par RACHID SAKASSA/ BADR-EDDINE BENZEMROUN
