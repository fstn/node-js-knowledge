# node-js-knowledge
node-js-knowledge


Attention au dépendence circulaire des requires (on obtient undefined)

Les promesses dans les tests:

    it('get() should not find ask before inserting ', function () {
            return RequestFacade.get(isbn, page, ex);
        }
    );
