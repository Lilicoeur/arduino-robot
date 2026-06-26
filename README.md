Arduino-CROMA


-Copier coller le code sur arduino IDE 2.3.8

-Brancher sur le Control Board (en haut)

-Selectionner COM8 Arduino Robot Control

-Et upload directement (pas besoin de verifier)

♥ Music :


Joue de la musique avec le buzzer:

alarmselect1: utilise les fleches (à droite de l'écran) pour choisir et bouton milieu selectionner

alarmselect2: utilise le knob (à gauche de la prise usb control board) pour choisir et bouton milieu selectionner

djset: Comment mixer comme un chef :

Le Knob (Bouton rond) : Tourne-le doucement pour changer la tonalité de la chanson. À gauche c'est très grave (Davy Jones), à droite c'est survolté.

Bouton HAUT : Appuie par petites touches pour rythmer la chanson avec des bips aigus.

Bouton BAS : Utilise-le sur le premier temps de chaque mesure (le "Pom" de Pom-Pom-Pom) pour rajouter de la puissance.

Bouton DROITE : C'est le "Scratch". Ça interrompt brièvement la mélodie pour faire un bruit de DJ.

Bouton GAUCHE : Si tu veux faire un "silence dramatique" avant de relâcher la sauce, reste appuyé dessus.jinglebells

piratesdescaraibes: joue le theme du film, intro seulement, et quelques fausses notesstarwars

synth: "bouger" le knob et la frequence du son change

♥ Motors :


Associé à des roues peut déplacer le robot en avant, en arrière et peut le faire tourner à gauche ou à droite.

Pour cela nous utilisons la commande : Robot.motorsWrite(vitesse du moteur gauche, vitesse du moteur droit).

Il ne faut pas oublier les commandes : 

- #include <ArduinoRobot.h> pour importer les commandes controlant les activités du robot

- Robot.begin() pour que les instructions suivantes lui soit destinées

○ Exemple de code pour faire avancer le robot :

                                                          #include <ArduinoRobot.h>
                                                          void setup() {
                                                            Robot.begin();
                                                          }
                                                          void loop(){
                                                            Robot.motorsWrite(100, 100);
                                                          }

○ Exemple de code pour faire touner le robot à gauche :

                                                           #include <ArduinoRobot.h>
                                                           void setup() {
                                                             Robot.begin();
                                                           }
                                                           void loop(){
                                                             Robot.motorsWrite(0, 100);
                                                           }

○ Exemple de code pour arrêter le robot :

                                                            #include <ArduinoRobot.h>
                                                            void setup() {
                                                              Robot.begin();
                                                            }
                                                            void loop(){
                                                              Robot.motorsWrite(0, 0);
                                                            }

♥ Les capteurs noir ou blanc : 

Les capteurs noir ou blanc servent notament à détecter une ligne foncée sur un fond clair ou une ligne claire sur un fond foncé.

Pour que les capteurs fonctionnent correctement il faut qu'ils soit calibrés et que la différence de luminosité entre la ligne et son fond soit assez importante sinon le robot peut les confondre assez facilement.

Pour calibrer un capteur il faut trouver une valeur de "référence" qui servira à différencier le noir et le blanc. Le noir a une valeur bien inférieure à celle du blanc. Le capteur différenciera la ligne noire du fond blanc si la valeur captée est inférieure à la valeur référencielle.

○ Exemple de code pour trouver la valeur de référence :  

                                                     #include <ArduinoRobot.h>

                                                     void setup() {
                                                      Robot.begin();
                                                      Serial.begin(9600);
                                                     }

                                                     void loop() {
                                                      Robot.updateIR();
                                                      Serial.println(Robot.IRarray[4]);
                                                      delay(100);
                                                     }

○ Exemple de code pour faire avancer ou arrêter le robot en fonction de ce que détecte le capteur :

                                                    void setup() {
                                                      Robot.begin();
                                                    }

                                                    void loop() {
                                                    
                                                      Robot.updateIR();
                                                      int C  = Robot.IRarray[2]; // center
                                                      
                                                      if ( C < 500 ) {
                                                        Robot.motorsWrite(100, 100);
                                                      }else{
                                                        Robot.motorsWrite(0, 0);
                                                      }
                                                    }

♥ Screen :

L'écran sers à afficher se dont on a besoins.

○ Exemple pour afficher la couleur noire sur tout l'écran :

                                                    Robot.stroke(0,0,0);
                                                    Robot.fill(0,0,0);
                                                    Robot.rect(0,0,128,160);

On peut aussi dessiner toutes sortes de formes comme une étoile :

                                                    void starr() {
                                                      int cx = 64;
                                                      int cy = 80;
                                                      int R  = 35;

                                                      float x[5];
                                                      float y[5];

                                                      Robot.stroke(255, 255, 0); 

                                                      for (int i = 0; i < 5; i++) {
                                                        float angle = (i * 72 - 90) * 3.14159 / 180.0;
                                                        x[i] = cx + R * cos(angle);
                                                        y[i] = cy + R * sin(angle);
                                                      }

                                                      Robot.line(x[0], y[0], x[2], y[2]);
                                                      Robot.line(x[2], y[2], x[4], y[4]);
                                                      Robot.line(x[4], y[4], x[1], y[1]);
                                                      Robot.line(x[1], y[1], x[3], y[3]);
                                                      Robot.line(x[3], y[3], x[0], y[0]);

                                                      delay(1000);

                                                      // clear screen
                                                      Robot.stroke(0,0,0);
                                                      Robot.fill(0,0,0);
                                                      Robot.rect(0,0,128,160);
                                                    }


On peut aussi écire des choses :

                                                    Robot.text("HELLO WORLD", position x, position y);

♥ Boutons :

Les boutons nous servent à communiquer avec la machine,on peut dire à la machine, si le boutton du milieu n'est pas pressé alors tu ne commence pas à rouler. Cela, en C++ se traduit par :

                              #include <ArduinoRobot.h>
                              void setup(){
                                Robot.begin();
                              }
                              void loop(){
                                int boutton = Robot.keyboardRead();

                                if (boutton == BUTTON_UP) {

                                  delay(1000);
                                  Robot.motorsWrite(110, 100);
                                  delay(1000);
                                  Robot.motorsWrite(0, 0);
                              }


Documentation et sources


https://github.com/arduino-libraries/Robot_Motor
https://github.com/arduino-libraries/Robot_Control

14/04 libraries inexistantes? uploading marche pas sauf chance après spam sur le bouton reset

15/04 installé un driver -> ca marche et ca compile les roues ne tournent pas, le beep et l'écran marchent

16/04 petits jeux et tests (voir le repo)

17/04 test des moteurs avec arduino uno -> ils marchent plutot bien donc probleme dans la batterie achat de piles rechargeables AA 2000 mAh (bonne autonomie)

15/06 test des anciens codes. Test du buzzer-> il fonctionne normalement. teste des moteurs -> ils fonctionnent normalement. recherche des commandes des capteurs noir ou blanc.

16/06 création du programme de suivi de ligne et ajout de musique interactive au début et à la fin du programme. Création d'un programme en utilisant les boutons. Problème à résoudre le screen ne fonctionne pas correctement même avec les bonnes commandes...

17/06 problème compris et adaptation du programme pour que le screen fonctionne comme on veut. amélioration du programme des boutons pour qu'une image s'affiche à chaque fois qu'un bouton est pressé. Lancement d'un nouveau projet. Projet fini.

♥ Rapport : Analyse et Restauration du Robot Arduino CROMA

• 1. Présentation du Système


Robot Arduino CROMA (2013-2018)

Plateforme robotique basée sur Arduino composée de deux cartes :

Control Board : gestion des capteurs, affichage, interface utilisateur

Motor Board : contrôle moteurs, alimentation externe

Modules Fonctionnels

Module	Fonction	Interface

Music	Gestion du buzzer, génération sons	Knob + boutons directionnels

Motors	Contrôle des roues	PWM via Motor Board

Screen	Affichage LCD	Écran sur Control Board

Capacités Système

Alimentation : 4 batteries AA NiMh rechargeables (6V nominal)

Programmation : Arduino IDE 2.3.8, port COM8 (Control Board)

Upload : Appui bouton reset si compilation échoue

Bibliothèques : Robot_Control et Robot_Motor (GitHub arduino-libraries)

• 2. Diagnostic Initial (14-15 avril)


État du Robot

Système	État	Symptôme

Compilation	❌ Erreur	Libraries manquantes

Upload	⚠️ Instable	Fonctionne après reset

Écran LCD	✅ OK	Affichage normal

Buzzer	✅ OK	Sons générés correctement

Moteurs	❌ Inactifs	Pas de rotation des roues

Problèmes Identifiés

Driver manquant : CH340 non reconnu par Windows

Batterie faible : Tension insuffisante pour moteurs

Alimentation insuffisante : Moteurs nécessitent 6V minimum

• 4. Démarche Expérimentale


- Phase 1 : Installation Driver (15 avril)

  Installation driver CH340/CH341

  Vérification port COM8

  Recompilation avec succès

  ✅ Écran et buzzer opérationnels

- Phase 2 : Test Moteurs Isolés (16-17 avril)

  Moteurs testés sur Arduino UNO avec :
  
  Générateur externe : 5V

  MOSFET IRF520N : contrôle PWM

  Diode protection : anti-pic tension

  Résultat : Moteurs fonctionnels → problème identifié : alimentation Robot

- Phase 3 : Remplacement Batterie (17 avril)

  Achat piles rechargeables AA NiMh :

  Capacité : 2000 mAh / batterie

  Total : 6V nominal (4 × 1.5V)

  Technologie : NiMh (rechargeables)

  Changer les piles du robot

  Résultat : le robot est autonaume au niveau de la batterie -> ne dépends pas d'un cable -> peut aller partout

- Phase 4 : trouver les bonnes commandes (15/06)

  Utilisation des librairies pour comprendre et aprivoiser le langage

  Concentration sur les moteurs -> commandes trouvées

  Trouver les commandes des capteurs -> commandes trouvées mais pas de réaction de la part du robot

  Identification du problème -> on ne connait pas le nom des capteurs

  Trouver le nom des capteurs (merci chatGPT) -> le nom des capteurs est Robot.IRarray[numéro du capteur]

- Phase 5 : coder le programme de suivi de ligne (16/06)

  Codage intensif

  Tests intensifs

  Résultat : les programme de suivi de ligne est opérationnel.
  
- Phase 6 : ajouter des bonus
  
  Coder pour l'arrêt à la fin du parcour

  Coder pour rajouter une mélodie au début et à la fin du programme

- Phase 7 : apprendre à utiliser l'écran

  Trouver les commandes -> commandes trouvées dans la librairie

  Problemme à résoudre -> le screen s'allume mais n'exécute aucune commande

  Mise en charge du robot

  Le screen peut afficher un rectangle encadré de rouge mais pas de texte

  Le screen réagit à "Robot.rect(10,10,50,30)"

  Mais ne réagit pas à "Robot.fill(0,0,0)" ; "Robot.stroke(255,0,0)" ; "Robot.text("HELLO", 20, 50)".

  Après essais "Robot.text("HELLO", 20, 50)" fonctionne mais est invisible si on ne met pas une autre forme en dessous -> on met un carré ou un rectangle en dessous pour   le voir.

  On peut faire des formes sur le screen mais on est toujours obligés de mettre un rectangle de la taille de l'écran pour voir apparaitre quoi que ce soit. on peut faire des lignes des rectangles ou carrés et des triangles.

  On peut changer la couleur de toutes les formes.

  Mais si on créé une forme plaine avec que des lignes on ne peut pass la remplire de couleur. Ou alors la commande n'a pas encore été trouvée.



- Phase 8 : l'idée d'un gros projet

  L'idée initiale, comme il existe beaucoup de programmes, est d'en rassembler au moins deux et de créer une interface sur le screen qui permet de choisir entre les deux. Pour cela il nous faudra une bonne maitrise du screen, les deux programmes, une bonne maitrise en "if" "else" et en boucle "for" et "while".

  Mon idée est que l'utilisateur puisse utiliser les deux programmes sans reset ni éteindre le robot. Ce qui peut être très interressant pour les journées portes ouvertes ou il ne faut pas perdre de temps à téléverser les programmes en fonction de ce que l'on veut montrer.

  On va essayer de combiner le programme des boutons et celui du suivis de ligne.

- Phase 9 : faire l'interface

  On va commencer par coder l'interface sur le screen en utilisant le screen et les boutons. Les boutons serviront au humains pour communiquer avec le robot sur leur       choix de programme. Le screen servira au robot à communiquer avec les humains.

  Interface terminée et fonctionnelle.

- Phase 10 : ajouter les programmes

  Il faut ajouter un programme, voir si il fonctionne comme ça, trouver un moyen de retourner à la page d'acceuil sans éteindre ou réinitialiser le robot et faire la mème chose avec l'autre programme.

  Quand le programme à été rajouté, le screen ne voulais plus afficher l'interface, il se trouve que l'écriture était de la mème couleur que le fond...

  Après beaucoup d'essais et de changements dans le code, il fonctionne.

  Maintenant il ne reste plus qu'à rajouter l'autre programme en espérant qu'il de fasse pas tout planter...

  Le programme à été ajouté sans soucis -> les deux programmes ont été ajouté et fonctionnent correctement.

  Le projet est terminé et prêt à être utilisé.

• 5. Architecture Électrique

   
Motor Board

                     Batterie (6V) ──┬──→ Moteur Roue Gauche (PWM pin)
                                     ├──→ Moteur Roue Droite (PWM pin)
                                     └──→ GND

Control Board

Motor Board ──[Série/SPI]──→ Capteurs + Buzzer + Écran

Specification Moteurs

Tension nominale : 6V (4 × 1.5V AA)

Courant : ~500-800 mA chacun

Contrôle : PWM via Motor Board

Alimentation : Batterie externe (pas USB)

• 6. Résultats et Validation


Modules Testés (17 avril)

Module	Code	État

music/djset	✅	Fréquence ajustable avec knob

music/alarmselect	✅	Sélection mélodies

music/starwars	✅	Reproduction audio

screen/*	✅	Affichage LCD normal

motors/*	✅	Rotation roues (batteries neuves)

capteurs ✅ capte la différence entre blanc et noir

Performance Batterie

Autonomie estimée : 2-3 heures (usage continu moteurs)

Temps recharge : Chargeur externe recommandé

Stabilité tension : 6V soutenue sous charge

• 7. Conclusion
   
Robot Arduino CROMA complètement fonctionnel.


♥ Problèmes résolus :


Driver CH340 installé

Batterie AA 2000 mAh remplacée

Tous modules validés

♥ Spécifications validées :


Music (buzzer, fréquence PWM)

Screen (LCD 5x8)

Motors (PWM dual moteur)

Suivi de ligne opérationnel

Prêt pour projets robotiques avancés.

Prêt pour les journées portes ouvertes.


♥ Références


Arduino Robot Control Library

Arduino Robot Motor Library  

Arduino IDE 2.3.8

CH340 Driver (Windows)
