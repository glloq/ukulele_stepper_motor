# ukulele_stepper_motor
> [!NOTE]
> inspiré par le youtuber steamHead avec sont ukulele avec quelques modification mecanique et l'ajout de MIDI

## Objectif 

Ce projet vise à créer un dispositif capable de jouer automatiquement un ukulele en réponse à des messages MIDI reçus via USB. 
Le dispositif utilise 4 moteurs pas à pas pour appuyer sur les cordes (frettes) et des moteurs ou des servomoteurs pour gratter les cordes , simulant ainsi le jeu d'un musicien.

le systeme est concu pour limiter au maximum la complexité et le cout du projet.

## Schema de principe 
![Schema principe](https://raw.githubusercontent.com/glloq/ukulele_stepper_motor/main/img/schemas%20principe%20base2.png?raw=true)

## Parties Mécaniques 

  ### Moteurs Pas à Pas (NEMA 17) :
  Permet le déplacement des "doigts" qui appuient sur les cordes à des positions spécifiques (frettes) en fonction des notes MIDI reçues.
     
  Composants associés :  
    Drivers de Moteurs Pas à Pas : TMC2208 pour un fonctionnement silencieux et une précision accrue grâce au microstepping.  
    Fin de course : Pour initialiser la position de chaque moteur lors du démarrage (homing).

  Système de Grattage des Cordes :
    Fonction : Mécanisme motorisé (servo ou moteur DC) pour gratter les cordes selon les messages MIDI.
    Nombre de systèmes : 4, un pour chaque corde du ukulele.

## Structure du code
Le code est structuré en plusieurs fichiers et classes, suivant une approche orientée objet pour une meilleure organisation et maintenabilité.

  ### settings.h 
- Mapping des notes MIDI pour chaque corde.
- Sens de rotation des moteurs pour le homing.
- Vitesse de déplacement des moteurs (homing et maximum).
- Pas par millimètre des moteurs pas à pas.
- Positions des frettes par rapport aux fins de course.

 ### Classe StepperMotor (stepperMotor.h / stepperMotor.cpp) :
- Gère les mouvements des moteurs pas à pas.
- Implémente des méthodes pour le homing, le déplacement vers une position donnée, et l'interrogation de l'état du moteur (position atteinte ou non).
Utilise la bibliothèque AccelStepper pour gérer l'accélération et la décélération.

### Classe Instrument (instrument.h / instrument.cpp) :
- Représente le ukulele dans son ensemble.
- Coordonne les mouvements des moteurs en réponse aux notes MIDI reçues.
- Implémente les fonctions noteOn et noteOff pour gratter les cordes quand le moteur est a position

### Classe MidiHandler (midiHandler.h / midiHandler.cpp) :
- Gère la réception et le traitement des messages MIDI via USB.
- Utilise la bibliothèque MIDI Library pour interpréter les messages MIDI et les relayer à l'instrument.
- Implemente des callbacks pour les événements MIDI comme noteOn, noteOff, etc.
