# Starting meeting
###### Who:
- Jisk, Ronald, Oscar (NLeSC)
- Joeri, Emily, Mike, Klim (ASTRON/UvA)

###### Where: Anton Pannekoek, UvA

###### When: 2016-07-12 11:30

## Topics
1. NLeSC setup
2. Accelerate: First talk about FRB Apertif processing pipeline (GPU) to get Jisk started
3. Access: Discussion with Mike Sipior (AA-ALERT model, broker, DB, etc.)

## 1. NLeSC Setup
- Ronald as backup / replacement Oscar
- Expected work load @ NLeSC first months:
  - Jisk 1.5 ~ 2 days a week (can go to Dwingeloo)
  - Ronald ~ 2 days a week
  - Oscar ~ 2 days a week (Not going to Dwingeloo until September)


## 2. Accelerate
Jisk needs to get started on the GPU, about half time: 2 - 2.5 day a week.

Content wise, the best is to get started with Alessio (PhD student with Rob).
Pipeline will run when the full cluster is placed: earliest sprint 2017
FPGA beam former should also be done but could be a delay.

ETA. 8 months.

Both CUDA / OpenCL support available at the moment, once chosen this is not really a requirement for Joeri

## 3. Access
Discussing/brainstorming AA-ALERT overview chart.
1. Observations from Apertif are processed by the GPU-based FRB Apertif processing pipeline (FRBAPP). The data format for the observational data is VO-based but we need to investigate how to store time information
2. If a FRB is found by the FRBAPP:
     - VOEvent is published to the FRB Broker (running @ ASTRON).
        - VOEvent definition may need to be extended for FRB-specific fields (example: detection confidence)
        - FRB Broker implemented with Comet (https://github.com/jdswinbank/Comet/) and set up be Mike already. Docker available.
     - Observational data is stored in ALTA archive @ ASTRON (8 PB capacity, disk+tape). Data throughput not critical (GB/s when observation is stored)
3. Other telescopes (LOFAR, UTMOST, CHIME, etc.) which are subscribed to the FRB Broker, receive notification (VOEvent) that follow-up is required
4. FRBCat (MySQL DB set up by Emily and running in Swinborne) is also subscribed to FRB Broker and stores all the FRBs in the DB (converter VOEvent to MySQL entries). FRBCat has a web interface for data exploration.
5. Manual publish of FRB events also possible
6. FRB Broker can also be subscribed to other brokers (by competition) and redirect possible events to FRBCat and to the follow-up telescopes

Important details
- Steps 1,2,3 (from observing/detecting FRB in Apertif to observing in a follow-up telescope) must be carried out in few seconds
- FRBCat (MySQL) contains at the moment 26 events (rate about 1 a month).
- Expected FRB detection rate: 1 per week per telescope
- In our project we do not deal with the archiving in ALTA
- FRBCat may contain a private part (more discussion regarding privacy, authentication and security is required)


## 4. Actions:

Related to Accelerate:
* Send background material to Jisk (JOERI)
* Contact Alessio (he works 2 days a week until January) (JISK)
Related to Access:
* Extend the VOEvent with FRB specifics, take inspiration from super novae. (OSCAR+JOERI+EMILY)
* Familiarize with Comet (RONALD+MIKE)
* Improve/update FRBCat DB schema (OSCAR+EMILY)
* Script for manually publishing VOEvent to FRB Broker (via web interface) (OSCAR+RONALD+EMILY)
* Script (daemon process) that is subscribed to FRB Broker and updates FRBCat DB (event hook) (OSCAR+RONALD)
* Script to publish detected FRB by GPU-based processing pipeline (FRBAPP) to the FRB Broker (OSCAR+RONALD+JISK)
* Define data format for Apertif FRB observations (add to VO format time domain information) JOERI+EMILY
* Define user requirements for improved FRBCat web interface (EMILY+JOERI)
* Improve web browsing of FRBCat (OSCAR+RONALD+EMILY)
