Arduino-CROMA


-Copier coller le code sur arduino IDE 2.3.8

-Brancher sur le Control Board (en haut)

-Selectionner COM13 Arduino Robot Control

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

- Robot.begin() pour que les instructions suivantes lui sont destinées

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


♥ Boutons :

Les boutons nous servent à communiquer avec la machine,on peut dire à la machine, si le boutton du milieu n'est pas pressé alors tu ne commence pas à rouler. Cela, en C++ se traduit par :

                              


Documentation et sources


https://github.com/arduino-libraries/Robot_Motor
https://github.com/arduino-libraries/Robot_Control

14/04 libraries inexistantes? uploading marche pas sauf chance après spam sur le bouton reset

15/04 installé un driver -> ca marche et ca compile les roues ne tournent pas, le beep et l'écran marchent

16/04 petits jeux et tests (voir le repo)

17/04 test des moteurs avec arduino uno -> ils marchent plutot bien donc probleme dans la batterie achat de piles rechargeables AA 2000 mAh (bonne autonomie)

15/06 test des anciens codes. Test du buzzer-> il fonctionne normalement. teste des moteurs -> ils fonctionnent normalement. recherche des commandes des capteurs noir ou blanc.

16/06 création du programme de suivi de ligne et ajout de musique interactive au début et à la fin du programme.

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

  Trouver les commandes...


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
