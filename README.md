# node-js-knowledge
node-js-knowledge


Attention au dépendence circulaire des requires (on obtient undefined)

Les promesses dans les tests:

    it('get() should not find ask before inserting ', function () {
            return RequestFacade.get(isbn, page, ex);
        }
    );
    
Les promesse imbriqués:
    
        var promise = new Promise(function(resolve,reject){

        data.entry.forEach(function (pageEntry) {
            if (pageEntry.messaging) {
                // Iterate over each messaging event
                 pageEntry.messaging.forEach(function (messagingEvent) {
                    return TeacherFacade.get(messagingEvent.sender.id)
                        .then(function (teacher) {
                            if (teacher != undefined) {
                                teacher = new TeacherModel("", "", messagingEvent.sender, []);
                                if (messagingEvent.optin) {
                                    _consumeAuthentication(messagingEvent, teacher).then(resolve,reject);
                                } else if (messagingEvent.message) {
                                     _consumeMessage(messagingEvent, teacher).then(resolve,reject);
                                } else if (messagingEvent.delivery) {
                                     _consumeDeliveryConfirmation(messagingEvent, teacher).then(resolve,reject);
                                } else if (messagingEvent.postback) {
                                     _consumePostback(messagingEvent, teacher).then(resolve,reject);
                                } else {
                                    reject("Webhook received unknown messagingEvent: ", messagingEvent);
                                }
                            } else {
                                reject(messagingEvent.sender.id + " is not a teacher");
                            }
                        });
                });
            }
        });
        }).then(function(success){
            /**
             * Log only if teacher
             */
            // Logger.log(messagingEvent);
            Logger.log("data " + JSON.stringify(data));
        });
        return promise;

Il est parfois difficile de trouver d'ou vient une erreur si on a oublié de reject une promesse,
pour cela on peut utliliser le code suivant:

    process.on('uncaughtException', function(err) {
        console.error("An uncaughtException was found, the program will end. " + err + ", stacktrace: " + err.stack);
    });
    
    
    process.on('unhandledRejection', function(err) {
        console.error("An unhandledRejection was found, the program will end. " + err + ", stacktrace: " + err.stack);
    });
