<html>
<head>
<title>Weather</title>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
<script>
    function gettingJSON(){
        document.write("jquery loaded");
        $.getJSON("http://api.openweathermap.org/data/2.5/weather?q=London&APPID=98c882cda3baf8ad9286ac9535773510",function(json){
            document.write(JSON.stringify(json));
        });
    }
    </script>
</head>
<body>
<button id = "getIt" onclick = "gettingJSON()">Get JSON</button>
</body>
</html>
