# Yipeng-L.github.io
<!DOCTYPE html>
<html lang="en">

<head>
    <style>
    input[type=text], select, textarea {
         width: 50%;
         padding: 20px;
         border: 1px solid #ccc;
         border-radius: 12px;
         resize: vertical;
    }
    .button {
         background-color: #008CBA;
         border: none;
         color: white;
         padding: 20px;
         text-align: center;
         text-decoration: none;
         display: inline-block;
         font-size: 16px;
         margin: 4px 2px;
         cursor: pointer;
}
        .button1 {border-radius: 12px;}
    
    </style>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">

    <!--https://code.jquery.com/jquery-3.3.1.js

https://code.jquery.com/jquery-3.3.1.min.js -->
    
    
    <title>Near Restaurant</title>
    <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Open+Sans:300,400"><!-- Google web font "Open Sans" -->
<!--
    <link rel="stylesheet" href="css/bootstrap.min.css">
	<link rel="stylesheet" href="css/jquery-ui.css">
-->
    <!--AIzaSyCHiPcn5TOQcCIGX8M6S68YBMKiLZP5emk-->
              <!-- AIzaSyBREuFDLAYSrxjj69koGsHLX_VejDMKWuE -->
    <!-- Main CSS -->
	<!-- google map -->
	<script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCHiPcn5TOQcCIGX8M6S68YBMKiLZP5emk&libraries=places"></script> <!--AIzaSyCHiPcn5TOQcCIGX8M6S68YBMKiLZP5emk-->
              <!-- AIzaSyBREuFDLAYSrxjj69koGsHLX_VejDMKWuE -->
	<script src="https://code.jquery.com/jquery-3.3.1.js"></script>
	<script src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.12.1/jquery-ui.min.js"></script>
    <!-- script src="js/templatemo-script.js"></script -->
<script>
var map;
var myCenter=new google.maps.LatLng(-37.8764368,145.0396434);
var location1;
var location2;
var location0;
var infowindow;  
var geocoder;   
    
function initialize() {
  geocoder = new google.maps.Geocoder();     
  var mapProp = {
    center: myCenter,
    zoom:15,
    mapTypeId: google.maps.MapTypeId.ROADMAP
  };
  map = new google.maps.Map(document.getElementById("googleMap"),mapProp); //��ʾ���map,�����Ӷ��map����
  
   var autocomplete = new google.maps.places.Autocomplete(document.getElementById('localtion'));
    google.maps.event.addListener(autocomplete, 'place_changed', function(){
        var place = autocomplete.getPlace();
        var location = place.formatted_address;
        location1 += place.geometry.location.A;
        location2 += place.geometry.location.F;
        location0 = "(" + location1 + ',' + location2 + ")";
    });
  //��ѯ�����Ĳ͹�
  var request =  {
	  location: myCenter,
	  radius:1000, // meter
	  types: ['restaurant']
  };

  //marker
  var marker=new google.maps.Marker({
	position: location0,
	animation:google.maps.Animation.BOUNCE, //�ɶ���ǩ
  });

  marker.setMap(map);

  //window
   infowindow = new google.maps.InfoWindow();

  var service = new google.maps.places.PlacesService(map);
  service.nearbySearch(request, callback);

} // initial end


function callback(results, status) {
	if (status == google.maps.places.PlacesServiceStatus.OK) {
		for (var i = 0; i < results.length; i++) {
			createMarker(results[i]);
		}
	}
}

function createMarker(place) {
	var marker = new google.maps.Marker({
        map: map,
        position: place.geometry.location
      });
     
	 google.maps.event.addListener(marker, 'click', function() {
        infowindow.setContent(place.name);
        infowindow.open(map, this);
      });

    /**function activatePlaceSearch(){
        var input = document.getElementById('localtion');
        var autocomplete = new google.maps.places.Autocpmplete(input);
    }*/
}
    
function codeAddress(){
	var address = document.getElementById('localtion').value;
    var center;
	geocoder.geocode( { 'address': address}, function(results, status){
		if (status == 'OK') {
            center = results[0].geometry.location;
			map.setCenter(results[0].geometry.location);
			var marker = new google.maps.Marker({
				map: map,
				position: results[0].geometry.location,
				animation:google.maps.Animation.BOUNCE //�ɶ���ǩ
			});
            
              //��ѯ�����Ĳ͹�
            var request =  {
	           location: center,
	           radius:1000, // meter
	           types: ['restaurant']
            };
            var service = new google.maps.places.PlacesService(map);
            service.nearbySearch(request, callback);
            
		}else{
				alert('Geocode was not successful for the following reason: ' + status);
		}
	});
}


// must be last executed.
google.maps.event.addDomListener(window, 'load', initialize);


</script>
</head>

<body>
    <div class="container">
	    <!-- Banner -->
		<div class="row">
            <div class="col-lg-12">
                <header class="text-center tm-site-header">
                    <div class="tm-site-logo"></div>
                    <h1 class="pl-4 tm-site-title">Food and Drinks Around You</h1>
                </header>
            </div>
        </div>
			<div class="row tm-section-mt">
               <div class="col-lg-12 mb-5">
					<!--form action="#contact" method="post" class="tm-contact-form"-->
                    <div class="row">
<!--                        <div class="form-group col-xl-4">-->
                            <input type="text" id="localtion" name="localtion" class="form-control" placeholder="Search Locations..." required/>
<!--                        </div>-->
<!--						<div class="form-group col-xl-4">-->
                            <button type="submit" class="button button1  pull-right" onclick="codeAddress()">Search</button>
<!--                        </div>-->
                    </div>
					<!--/form-->
				</div>
                <div id="googleMap" style="width:80%;height:500px;">
                </div>

        <hr>
        <!-- Footer -->
        <footer class="row mt-5 mb-5">
            <div class="col-lg-12">
                <p class="text-center tm-text-gray tm-copyright-text mb-0">Copyright &copy;
                    <span class="tm-current-year">2018</span> T-Watch Ltd.  | Designer: <a href="" class="tm-text-white">Yipeng Li</a> 
                </p>
            </div>
        </footer>
    </div>
    <!-- .container -->

</body>
</html>
