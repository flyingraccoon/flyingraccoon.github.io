<html>
<body>
    <h1>Display OpenWeatherMap API data in a jQuery DataTables</h1>
    <h2>Important Link - <a href="http://www.yogihosting.com/jquery-datatables/">jQuery DataTables</a></h2>
    <h3><button id="download">Download »</button></h3>
    <div id="content">
        <h3>Click the button to get Weather Information of different Cities</h3>
        <div class="textAlignCenter">
            <button id="submitButton">Get Weather</button>
            <table id="weatherTable"></table>
        </div>
    </div>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.0/jquery.min.js"></script>
    <script type="text/javascript" src="https://cdn.datatables.net/1.10.16/js/jquery.dataTables.min.js"></script>

    <script type="text/javascript">
        $(document).ready(function () {
            $("#reset").click(function (e) {
                location.reload();
            });

            $("#submitButton").click(function (e) {
                $.ajax({
                    type: "POST",
                    url: "http://api.openweathermap.org/data/2.5/group?q=London&appid=98c882cda3baf8ad9286ac9535773510&units=metric",
                    dataType: "json",
                    success: function (result, status, xhr) {
                        res = CreateWeatherJson(result);
                        $("#weatherTable").append("<thead><tr><th>City Id</th><th>City Name</th><th>Temperature</th><th>Min Temp</th><th>Max Temp</th><th>Humidity</th><th>Pressure</th></thead></table>");
                        $('#weatherTable').DataTable({
                            data: JSON.parse(res),
                            columns: [
                                { data: 'cityId' },
                                { data: 'cityName' },
                                { data: 'temp' },
                                { data: 'tempMin' },
                                { data: 'tempMax' },
                                { data: 'pressure' },
                                { data: 'humidity' }
                            ],
                            "pageLength": 5
                        });
                    },
                    error: function (xhr, status, error) {
                        console.log("Error: " + status + " " + error + " " + xhr.status + " " + xhr.statusText)
                    }
                });
            });

            function CreateWeatherJson(json) {
                var newJson = "";
                for (i = 0; i < json.list.length; i++) {
                    cityId = json.list[i].id;
                    cityName = json.list[i].name;
                    temp = json.list[i].main.temp
                    pressure = json.list[i].main.pressure
                    humidity = json.list[i].main.humidity
                    tempmin = json.list[i].main.temp_min
                    tempmax = json.list[i].main.temp_max

                    newJson = newJson + "{";
                    newJson = newJson + "\"cityId\"" + ": " + cityId + ","
                    newJson = newJson + "\"cityName\"" + ": " + "\"" + cityName + "\"" + ","
                    newJson = newJson + "\"temp\"" + ": " + temp + ","
                    newJson = newJson + "\"pressure\"" + ": " + pressure + ","
                    newJson = newJson + "\"humidity\"" + ": " + humidity + ","
                    newJson = newJson + "\"tempMin\"" + ": " + tempmin + ","
                    newJson = newJson + "\"tempMax\"" + ": " + tempmax
                    newJson = newJson + "},";
                }
                return "[" + newJson.slice(0, newJson.length - 1) + "]"
            }
        });
    </script>
</body>
</html>
