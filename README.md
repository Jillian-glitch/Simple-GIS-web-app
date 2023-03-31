# Simple-GIS-web-app using openlayers, Html, css and Javascript. The web application was created to showcase Kenya bareareas and also show how to add various web app functionalities such as zoom, addLocation etc.
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web Mapping Application</title>
    <link rel="stylesheet" href="resources/ol/ol.css">
    <link rel="stylesheet" href="resources/layerswitcher/ol-layerswitcher.css">
    <link rel="stylesheet" href="style.css">
</head>

<body>
    <div id="map"></div>
    <div id="popup" class="ol-popup">
        <a href="#" id="popup-closer" class="ol-popup-closer"></a>
        <div id="popup-content"></div>
    </div>

    <div class="attQueryDiv" id="attQueryDiv">
        <div class="headerDiv" id="headerDiv">
            <label for="">Attribute Query</label>
        </div><br>
        <label for="">Select Layer</label>
        <select name="selectLayer" id="selectLayer">
        </select><br><br>

        <label for="">Select Attribute</label>
        <select name="selectAttribute" id="selectAttribute">
        </select><br><br>

        <label for="">Select Operator</label>
        <select name="selectOperator" id="selectOperator">
        </select><br><br>

        <label for="">Enter Value</label>
        <input type="text" name="enterValue" id="enterValue">
        </select><br><br>

        <button type="button" id="attQryRun" class="attQryRun" onclick="attributeQuery()">Run</button>
    </div>

    <div class="attListDiv" id="attListDiv">
    </div>

    <script src="resources/ol/ol.js"></script>
    <script src="resources/layerswitcher/ol-layerswitcher.js"></script>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="main.js"></script>
</body>

</html>

#CSS
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body{
    /* margin: 0; */
    /* padding: 0; */
    overflow: hidden;
}

#map {
    position: absolute;
    width: 100vw;
    height: 100vh;
}

.mousePosition {
    position: fixed;
    top: auto;
    right: auto;
    bottom: 5px;
    left: 85%;
    /* transform: translateX(-50%); */
    /* border: 1px solid grey; */
    border-radius: 10px;
    padding: 5px;
    /* background-color: rgba(225, 225,225, 0.4); */
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 12px;
    font-weight: bolder;
}

.ol-popup {
    position: absolute;
    background-color: white;
    box-shadow: 0 1px 4px rgba(0,0,0,0.2);
    padding: 15px;
    border-radius: 10px;
    border: 1px solid #cccccc;
    bottom: 12px;
    left: -50px;
    min-width: 280px;
  }
  .ol-popup:after, .ol-popup:before {
    top: 100%;
    border: solid transparent;
    content: " ";
    height: 0;
    width: 0;
    position: absolute;
    pointer-events: none;
  }
  .ol-popup:after {
    border-top-color: white;
    border-width: 10px;
    left: 48px;
    margin-left: -10px;
  }
  .ol-popup:before {
    border-top-color: #cccccc;
    border-width: 11px;
    left: 48px;
    margin-left: -11px;
  }
  .ol-popup-closer {
    text-decoration: none;
    position: absolute;
    top: 2px;
    right: 8px;
  }
  .ol-popup-closer:after {
    content: "✖";
  }

  .myButton {
    background-color:rgba(22, 85, 167, 0.6);
    color: white;
    font-weight: 700;
    text-align: center;
    height: 25px;
    width: 25px;
    border-radius: 2px;
    border: none;
    align-items: center;
}

.myButton:hover{
  background-color: rgb(0, 60, 136, 1);
}

.myButton.clicked{
  background-color: rgba(22, 170, 35, 0.8);
}

.myButton.clicked:hover{
background-color: rgba(22, 170, 35, 1);
}

.homeButtonDiv{
  position: relative;
  display: block;
  margin: 10px auto auto 10px;
}

.fsButtonDiv {
  position: relative;
  display: block;
  margin: 1px auto auto 10px;
}

.featureInfoDiv {
  position: relative;
  display: block;
  margin: 1px auto auto 10px;
}

.lengthButtonDiv {
  position: relative;
  display: block;
  margin: 1px auto auto 10px;
}

.areaButtonDiv {
  position: relative;
  display: block;
  margin: 1px auto auto 10px;
}

  /* start : Measure styling */

  .ol-tooltip {
    position: relative;
    background: rgba(0, 0, 0, 0.5);
    border-radius: 4px;
    color: white;
    padding: 4px 8px;
    opacity: 0.7;
    white-space: nowrap;
    font-size: 12px;
  }
  .ol-tooltip-measure {
    opacity: 1;
    font-weight: bold;
  }
  .ol-tooltip-static {
    background-color: #ffcc33;
    color: black;
    border: 1px solid white;
  }
  .ol-tooltip-measure:before,
  .ol-tooltip-static:before {
    border-top: 6px solid rgba(0, 0, 0, 0.5);
    border-right: 6px solid transparent;
    border-left: 6px solid transparent;
    content: "";
    position: absolute;
    bottom: -6px;
    margin-left: -7px;
    left: 50%;
  }
  .ol-tooltip-static:before {
    border-top-color: #ffcc33;
  }

  /* end : Measure styling */

  .ol-dragbox {
    border: 2px dashed grey;
    background-color: rgba(200, 200, 200, 0.6);
  }

  .ziButtonDiv {
    position: relative;
    display: block;
    margin: 1px auto auto 10px;
  }

  .zoButtonDiv {
    position: relative;
    display: block;
    margin: 1px auto auto 10px;
  }

  .myButtonDiv {
    position: relative;
    display: block;
    margin: 1px auto auto 10px;
  }

  .attQueryDiv {
    position: absolute;
    display: none;
    margin: 70px auto auto 45px;
    /* padding: 10px; */
    width: 250px;
    /* height: 250px; */
    background-color: rgba(255, 255, 255, 0.9);
    font-family: inherit;
    font-size: 1em;
    line-height: 1.45;
    border-radius: 2px;
    border: 1px solid #d1d1d1;
    box-shadow: 0px 0px 15px rgba(252, 213, 213, 0.5);
    /* z-index: 4; */
    /* overflow: scroll; */
  }
  
  #headerDiv{
    background-color: midnightblue;
    height: 35px;
  }
  
  #headerDiv label {
  color: white;
  }

  .attQueryDiv select {
    width: 225px;
    height: 35px;
    border: 1px solid lightgrey;
    border-radius: 3px;
    margin: 0px auto 0px 10px;
  }
  
  .attQueryDiv input {
    width: 225px;
    height: 35px;
    border: 1px solid lightgrey;
    border-radius: 3px;
    margin: 0px auto 0px 10px;
  }
  
  .attQueryDiv label {
    color: grey;
    margin: 10px 10px 0px 10px;
  }
  
  .attQueryDiv label:nth-child(1) {
    margin: 10px;
    font-weight: bold;
   }
  
  .attQryRun {
    color: white;
    background-color: green;
    border-radius: 3px;
    border: 1px solid darkgreen;
    height: 35px;
    width: 50px;
    margin: 10px;
  }

  .attListDiv {
    position: absolute;
    display: none;
    margin: 350px auto auto 350px;
    /* padding: 10px; */
    width: auto;
    max-width: 750px;
    /* height: auto; */
    /* max-height: 250px; */
    height: 250px;
    background-color: rgba(255, 255, 255, 0.9);
    font-family: inherit;
    font-size: 1em;
    line-height: 1.45;
    border-radius: 2px;
    border: 1px solid #d1d1d1;
    box-shadow: 2px 3px 2px rgba(0, 0, 0, 0.5);
    /* z-index: 5; */
    overflow: scroll;
  }
  
  #attQryTable {
    padding: 0;
    border-collapse: collapse;
  }
  
  #attQryTable, td, th {
    padding: 0px 5px 0px 5px;
    border: 1px solid black;
  }
  
  #attQryTable th {
    color: white;
    background-color: midnightblue;
    position: sticky;
    top: 0;
    font-style: normal;
    padding: 5px 10px 5px 10px;
  }

  #attQryTable tr:hover
  /* {font-weight: bolder; background-color: rgb(225, 225, 225);} */
  {background-color: lightgrey;}
