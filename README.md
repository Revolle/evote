# evote
Specification for *evote* system, for large community.

## Constraints :
- Distributed ( no central authority/system) 
- Unanimous ( all of its users participate on the validation )
- Immutable ( vote result cannot be altered )
- Confidential (nobody can extract a listing of "who vote what")
- transparency (all of its users can verify the result, and his own particpation in the vote)
- no physical influence during the vote (control that nobody influence the vote, like in a voting booth)

## Pre-requisit :
- external authentication mechanism (eg. FranceConnect)

## Main architecture :

Two layers of distributed ledger : 
- many micro-distibuted-ledgers "*e-electoral-office*" : similar to a temporary virtual small electoral office. Created "on the fly", to build a transaction of votes. An app manages this *e-electoral-office*. The communication between users of a *e-electoral-office* is only peer-to-peer and encrypted between users.
- one *vote-block-chain* : stores the "vote-counting" per *e-electoral-office*. An app authorises to insert vote-counting in the *vote-block-chain*, verify vote-counting blocks using an id, calculate the result of the vote (summing the count-votings blocks).

One central *authentication-system* :
- has the list of *vote-block-chain*
- authenticate the users
- per *vote-block-chain*, logs the status of the user (*not-yet-voted*, *voting*, *has-voted*), delivers a unique-id and his face-photo to the *e-electoral-office*, logs the valid *transaction-unique-id* delivered by the *vote-block-chain* (without link to the user)

### e-electoral-office use-case :
Starting the *e-electoral-office* app, the user makes an authentication on the external *authentication-system* and the *vote-block-chain* he needs. If the users has the status *not-yet-vote*, the *authentication-system* sends back a unique-id, his face-photo, and set his status as *voting*.
If the users leaves the app before the end of protocol, the app sends-back status *not-yet-vote*.
The app searches on the net an available and valid *e-electoral-office* linked to the *vote-block-chain*. If there is no *e-electoral-office*, the app creates one. Else, app registers randomly to the existing and valid *e-electoral-office*.
When a minimum of users are registered on the *e-electoral-office*, the voting protocole can start. This minimum must be more than 3 times the number of choices, and less than a "certain" value to avoid delay between the registration in the *e-electoral-office* and the start of the voting.
To vote, the user records a self-video, telling his choice, and making 360° panoramic view to show that is is alone. He confirms his vote clicking on the right check-box.
The app checks the face on the video against the face provided by the *authentication-system*. If the face control is correct, the face on the video is hidden. Else, the user must restart his vote.
The user's vote (video without face, and choice) is sent to two users part of his *e-electoral-office*. If the two users validates the vote (panoramic view, oral-choice=digital-choice), the vote is set as *correct* in the *e-electoral-office*. Else the vote is sent to another user, up to have two validations. If it is impossible to have a validation, the user can choose to restart his vote, ot to leave and change to another *e-electoral-office*.
When all votes are *correct* in the *e-electoral-office*, the list of user's-id/vote is sent to all the users of the *e-electoral-office*.
If all participants validate the list, the list of votes (without user's id) is added as a transaction in the *vote-block-chain*. The *transaction-unique-id* delivered by the *vote-block-chain* is returned to the app.The app sends back to *authentication-system* the status *has-voted* for each user, and the *transaction-unique-id*. 
If the participants does not validate the list, the *e-electoral-office* is declared as non-valid and cancelled , and app sends back to *authentication-system* the status *not-yet-voted* for each user..
The end-user receives the *transaction-unique-id* delivered by the *vote-block-chain*.

### vote-block-chain :
Each *e-electoral-office* inserts one transaction in the *vote-block-chain*. This transaction contains the counting of votes (and only this), made in this *e-electoral-office*, and a *transaction-unique-id*, known by the users who vote in this *e-electoral-office*.
A user can verify that the his transaction (the one he validated in his *e-electoral-office*) is available in the *vote-block-chain*.
A user can count the global result of the vote, reading and summing the counts of each transaction in the *vote-block-chain*. Nobody can have an overview of "who vote wha"t. Nobody can alter the voting.
