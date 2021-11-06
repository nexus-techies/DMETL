# DMETL

@@@@ WORKING WITH XML, QUEUES AND WEB-SERVICES

1. Using tXMLMap to read XML
2. Using tXMLMap component to create XML Document
3. Reading Complex Hierarchical XML
4. Writing Complex XML
5. Calling a SOAP web service
6. Calling a RESTful web service
7. Reading and writing to a queue
8. Ensuring Lossless queue using sessions


@ Purpose:
WORKING WITH XML, QUEUES AND WEB-SERVICES

- Create metadata/schema files like xml, excel, csv, etc directly from METDAATA section or EDit schema and drag and drop first.....


@ Components used:
1. tXMLMap : Joins.
2. XPATH 
Used to decompose complex XML into more manageable chunks.
- contains : element [<customers>] , attribute:value [lang="en"], text [Harry Potter]
3. tXMLOutput, tWriteXMLField
Used to create complex multilevel XML structures from flat structures.
4. Web services:
SOAP, RESTFUL
5. Message Queues :
ActiveMQ


====================================================================================================================================================

1. Using "tXMLMap" to read XML

@ Purpose:
- Shows how we can convert XML record saved in a file into a format that is readable by "tXMLMap"
AND how can we read and process the data in XML record.


@ Components used:
1. tFileInputXML : XML file
2. tXMLMap : Joins.
3. tLogRow : display flat(table) Output on console


@ WORKING:
- The "tFileInputXML" component uses "XPATH" to convert the input XML into a JAVA Document object, so that an XML tree can be created by the "tXMLMap" component.
- By pointing to "tXMLMap" at our XML file , we were able to import the XML structure from the file.
So that we could manipulate the fields individually.
- Once the document tree has been defined, elements can then be copied from the output to the input as with a normal "tMap."

-------------------------------------------
@ STEPS

- Create a new job design 

[I] Convert XML file into JAVA document for use by downstream component.
i. Drag and drop a "tFileInputXML" into job design panel.
ii.Click on it ---> Go to Component window --> 
- Edit the schema and add a col named "payload".
- Make it of type "Document" --> save
iii. Click on "tFileInputXML" and change "file/stream" path to context.cookbookData + "/chapter9/chapter09_jo_0010_customerData.xml".
iv. Change the "Loop Xpath query" field to "/".
v. Add an "XPath query of "." , and tick the box Get Nodes.
vi. ..


vii. Add "tXMLMap" component to the canvas and link it to the "tFilteInputXML" component.
viii. Open the "tXMLMap" --> MapEditor --> component and right click on "payload".
ix. Select import from file:
x. Navigate to the input in the folder for this chapter and when you select the file you will see that the XML structure has now been added to the "tXMLMap" components input table.

xi. Add an output table[By clicking on the "+" button on the RHS box in "tXMLMap"s MAP EDITOR] ->  named "customerOut", and drag the fields from the input to the output.
 Your "tXMLMap" ....
xii. Add a "tLogRow" to the job, connect it to the output of the "tXMLMap" and then run the job. 
You will see that the data has now been flattened into a normal Talend row.

====================================================================================================================================================

2. Using tXMLMap component to create XML Document


@ Purpose:
- Here, we will be reading from a flat file and converting it to a XML document for output file.


@ Components used:
1. tFileInputDelimited
2. tXMLMap
3. tFileOutputXML


@ WORKING:
- Defining the output type of "Document" allows us to define an XMLformat within the "tXMLMap" component into which we can then map our input data.
- the "tFileOutputXML" component by default will create an XML structure from a normal schema; HOWEVER, it is possible to force it to handle a document.

-------------------------------------------
@ STEPS

- Create a new job design 

[I] Convert the input document into a JAVA document which can store the XML.
i. Drag the "tXMLMap" component and link the "tFileInputDelimited" component to it.
ii. Create an output table named "customerDocumentOut" and add a field named "payload". 
Make the field type of "Document"
iii. You will see that the field in the output table has changed to become a simple structure
iv. Retriev the XML format from the file containing our target XML structure
v. Drag the fields from input to output, and set the "countryOfResidence" component to "UK"
vi. txLMAP : mapEditor view
vii. Add "tFileOutputXML" and connect it to the "tXMLMap" COMPONENT.
viii. Open/Click "tFileOutputXML" and 
- tick "incoming record is a document" and set the File name to "outCustomerData.xml" .
ix. Run the job

====================================================================================================================================================


3. Reading Complex Hierarchical XML


@ Purpose:
- It shows how can we deconstruct a more complex XML record into individual sets of data while ensuring that the hierarchical relationships between the data are not lost.

@ Components used:
1. tFileInputXML
2. tLogRow


@ WORKING:
- The XML schema component alllows us to map data from the XML structure into a flattend Talend schema easily, ready for use in the downstream componets. 
- First, we defined an XML schema to extract just the "customer" fields.
- Then, we did the same for the "order" fields, but remebered to also extract the key for the "customer", which is "customerId".  
This will ensure that for each set of order data of which there are two, the "customerID" is present.
- We Repeated this process again for the "orderItem" fields, remembering to extract the "customer" and "order" fields.
- Finally we dragged the schemas to the canvas, linked them and added "tLogRow" outputs.



-------------------------------------------
@ STEPS

- Create a new job design 

[I] First we will create a customer schema using the XML schema wizard.
i. In the metadata panel under "File XML" , Right click and click the option "Create file XML".
ii. Name the XML file "XMLOrderDataCustomer".
iii. Select input XML , then click Next
iv. Click on Browse to select the XML file ""
v. In the "Schema viewer" , you can check that this is the correct file , then click NEXT.
vi. Drag the field "customer" from the "Source Schema" panel to the "Target Schema" panel and "Xpath loop expression".
vii. Drag "name" and "age" to the "Tagrget Schema" panel "Fields to extract".
viii. Drag the field "customer" from the "Refresh Preview" panel and you will see the values as they appear in the schema.
ix. Next
x. The next step allows you to validate and change the extracted field names and lengths if you need to. 
xi. Click "Finish" to complete the schema.

@ Creating order schema
xi. Repeat the steps above for a schema named "order"....
xii. Map the "customerId" and the "order" fields so that your mapping looks like the one in the screenshot.

@ Creating order item schema
xiii. Repeat the steps above for a schema named "OrderItem".
xiv. Map the "customerId", "orderNumber", and other order item fields so that your mapping looks like the one in the screenshot.

@ Adding to the job
xv. Finally drag all three scheams to the canvas , Selecting the component type of "tFileInputXML", join them up one above other using "subjob Ok".
xvi. Connect corresponding "tFileOutputExcel" file components to the Xmlinputs.
xvii. Run the JOB.

- You can see the Excel files filled with the data inputted from the hierarchical nested XML file that we mapped...
