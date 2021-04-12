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
- one *vote-block-chain* : stores the "vote-counting" per *e-electoral-office*. stores the events when a user or a *e-electoral-office* changes their status.

One central *authentication-system* :
- has the list of *vote-block-chain*
- authenticate the users
- delivers a *user-unique-id* and user's face-photo to the *e-electoral-office*

### e-electoral-office use-case :

Starting the *e-electoral-office* app, the user makes an authentication on the external *authentication-system* and the *vote-block-chain* he needs. If the users has not the status *voting* in the *vote-block-chain*, the *authentication-system* sends back a *user-unique-id* and his face-photo. This *user-unique-id* is unique for the couple (user, *vote-block-chain*) and not reversible to the user. This *user-unique-id* is delivered to the user. This *user-unique-id* can be used by the user to check his status in the block-chain. The app writes in the *vote-block-chain* the status *voting* for this *user-unique-id*.

If the users leaves the app before the end of protocol, the app writes in the *vote-block-chain* the status *not-yet-vote* for this *user-unique-id*.
If there is no *e-electoral-office-id*  with status *pending* in the *vote-block-chain*, the app creates an *e-electoral-office-id* in the *vote-block-chain* with status *pending*. The app registers randomly to one of the existing *e-electoral-office-id* with status *pending*.

When a minimum of users are registered on the *pending* *e-electoral-office*, the voting protocole can start. This minimum should be more than 3 times the number of choices, and less than a "certain" value to avoid delay between the registration in the *e-electoral-office* and the start of the voting. The status of the *e-electoral-office* id added in *vote-block-chain* as *voting*. The *e-electoral-office-id* is delivered to the user. This *e-electoral-office-id* can be used by the user to check the transaction of his *e-electoral-office* in the block-chain. 

To vote, the user records a self-video, telling his choice, and making 360° panoramic view to show that is is alone. He confirms his vote clicking on the right check-box.
The app checks the face on the video against the face provided by the *authentication-system*. If the face control is correct, the face on the video is hidden. Else, the user must restart his vote.

The user's vote (video without face, and choice) is sent to two users part of his *e-electoral-office*. If the two users validates the vote (panoramic view, oral-choice=digital-choice), the vote is set as *correct* in the *e-electoral-office*. Else the vote is sent to another user, up to have two validations. If it is impossible to have a validation, the user can choose to restart his vote. The user can also leave from this *e-electoral-office* (the app writes in the *vote-block-chain* the status *not-yet-vote* for the *user-unique-id*).

When all votes are *correct* in the *e-electoral-office*, the list of user's-id/vote is sent to all the users of the *e-electoral-office*.
If all participants validate the list, the *e-electoral-office* is declared as *voted* in the *vote-block-chain*, with the list of votes (without *user-unique-id*). The app writes in the *vote-block-chain* the status *voted* for the *user-unique-id*. 

If the participants does not validate the list, the *e-electoral-office* is declared as *non-valid* in the *vote-block-chain*, and the app writes in the *vote-block-chain* the status *not-yet-vote* for the *user-unique-id*.

### vote-block-chain :

The *vote-block-chain* stores these events :
- change status of *e-electoral-office* , identified with a *e-electoral-office-id* ( *pending* , *voting*, *voted* with the the anonymous result of the votes)
- change status of user, identified with a *e-electoral-office*-id delivered by *authentication-system* ( *voting* , *noy_yet_vote*, *voted* )

A user can verify his status (using his *user-unique-id* )and his transaction ( using his *e-electoral-office-id*).

A user can count the global result of the vote, reading and summing the counts of *e-electoral-office-id* with status *voted* in the *vote-block-chain*.
