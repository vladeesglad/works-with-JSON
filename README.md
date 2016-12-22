# works-with-JSON

THE initial impetus for the creation of this class and method... pretty simple.  I was looking for an easy, low-cost method to dynamically extract the values of field values from records.  No finite list of field keys... just a string of field values required and generated at runtime.

I didn't find anything in Apex that was directly intended to support this function, so I built this.  Since then, of course, salesforce has this wonderful new method getPopulatedFieldsAsMap(), which, of course, fits the bill perfectly, obviating the need for this.

You could also roll this functionality by calling a describe on the object, collecting the keys for all the object's fields, and then looping through the fields to inquire if the record in context has a value for that field; if it does... add it to the map.  Seemed like a lot of work to me, especially in a context where we were executing a fairly complex function that needed to provide something like global access to a record during the execution context.  So we did this:

(1)   Queried and obtained the record that was required
(2)   called JSON.serialize() on the record
(3)   saved the serialized record as a public static string in a class that was designed to hold and provide universal access to context variables;
(4)   called JSONextract.getFromJson() to extract specific field values...

        jsonExtract jrec = new jsonExtract();
        jrec.jsonstring = JSON.serialize(sObjectRecord);
        jrec.objname = 'account';
        
        jrec.getFromJson('Name');

