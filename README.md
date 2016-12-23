# works-with-JSON

THE initial impetus for the creation of this class and method... pretty simple.  I was looking for an easy, low-cost method to dynamically extract the values of field values from records.  No finite list of field keys... but a string of field keys (labels or api names) required and generated at runtime.

I didn't find anything in Apex that was directly intended to support this function, so I built this.  Since then (2013), of course, salesforce has this wonderful new method getPopulatedFieldsAsMap(), which fits the bill perfectly, obviating the need for this.

You could also roll this functionality by (1) creating an empty map, (2) calling a describe on the object, (3) collecting the keys for all the object's fields, and (4) looping through the record's fields to inquire if that record in context has a value for that field; if it does... (5) add it to the map created in step (1).  This seemed like a lot of work to me, especially in a context where we were executing a fairly complex function that needed to provide something like global access to the record during the entire execution context.  So we did this:

(1)   Query and obtain the required record;

(2)   call JSON.serialize() on the record;

(3)   save the serialized record as a public static string in a class intended to hold and provide universal access to context variables;

(4)   call JSONextract.getFromJson() to extract specific field values...

        jsonExtract jrec = new jsonExtract();
        jrec.jsonstring = JSON.serialize(sObjectRecord);
        jrec.objname = 'account';
        
        jrec.getFromJson('Name');

