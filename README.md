# JSON_POWERDB-
Simple  form page that takes the details from the user and store it in the JSON_POWERDB.
 <!DOCTYPE html>

<html lang="en">
<head>
<title>Bootstrap Example</title>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet"
href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
<script
src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script
src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
<style>
    h2{
        background-color: lightgray ;
    }
   
</style>
</head>
<body>
<div class="container">
<h2>Enter Customer Details :</h2>
<form id="CusForm" method="post">
<div class="form-group">
<span><label for="cusId">Customer ID:</label> <label id="CusIdMsg">
</label></span>
<input type="text" class="form-control" name="CusId" id="CusId"
placeholder="Enter Customer ID" required>
</div>
<div class="form-group">
<label for="CusName">Customer Name:</label>
<input type="text" class="form-control" id="CusName"
placeholder="Enter Customer Name" name="CusName">
</div>
<div class="form-group">
<label for="CusEmail">Email:</label>
<input type="email" class="form-control" id="CusEmail"
placeholder="Enter Employee Email" name="empEmail">
</div>
<input type="button" class="btn btn-primary" id="CusSave" value="Save"
onclick="saveCustomer();">
</form>
</div>
<script>
$("#CusId").focus();
function validateAndGetFormData() {
var CusIdVar = $("#CusId").val();
if (CusIdVar === "") {
alert("Customer ID Required Value");
$("#CusId").focus();
return "";
}
var CusNameVar = $("#CusName").val();
if (CusNameVar === "") {
alert("Customer Name is Required Value");
$("#CusName").focus();
return "";
}
var CusEmailVar = $("#CusEmail").val();
if (CusEmailVar === "") {
alert("Customer Email is Required Value");
$("#CusEmail").focus();
return "";
}
var jsonStrObj = {
CusId: CusIdVar,
CusName: CusNameVar,
CusEmail: CusEmailVar,
};
return JSON.stringify(jsonStrObj);
}
// This method is used to create PUT Json request.
function createPUTRequest(connToken, jsonObj, dbName, relName) {
var putRequest = "{\n"
+ "\"token\" : \""
+ connToken
+ "\","
+ "\"dbName\": \""
+ dbName
+ "\",\n" + "\"cmd\" : \"PUT\",\n"
+ "\"rel\" : \""
+ relName + "\","
+ "\"jsonStr\": \n"
+ jsonObj
+ "\n"
+ "}";
return putRequest;
}
function executeCommand(reqString, dbBaseUrl, apiEndPointUrl) {
var url = dbBaseUrl + apiEndPointUrl;
var jsonObj;
$.post(url, reqString, function (result) {
jsonObj = JSON.parse(result);
}).fail(function (result) {
var dataJsonObj = result.responseText;
jsonObj = JSON.parse(dataJsonObj);
});
return jsonObj;
}

function resetForm() {
$("#CusId").val("")
$("#CusName").val("");
$("#CusEmail").val("");
$("#CusId").focus();
}
function saveCustomer() {
var jsonStr = validateAndGetFormData();
if (jsonStr === "") {
return;
}
var putReqStr = createPUTRequest("90937972|-31949265773211043|90953606",
jsonStr, "Customer", "Customer-Rel");
alert(putReqStr);
jQuery.ajaxSetup({async: false});
var resultObj = executeCommand(putReqStr,
"http://api.login2explore.com:5577", "/api/iml");
alert(JSON.stringify(resultObj));
jQuery.ajaxSetup({async: true});
resetForm();
}
</script>


</body>
</html>
