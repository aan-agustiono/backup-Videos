<!DOCTYPE html>
<html>
  <head>
    <title>Video Player</title>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="assets/css/XP0.css" />
    <style>	
	/* buat column */	
	.colm_a { float: left; width: 65%; }
	.colm_b { float: left; width: 35%; }
	/* Clear floats after the columns */
	.row:after { content: ""; display: table; clear: both; }	
		
	/* windows variable display */
	#rightcolumn { float: right; height: 450px; width: 100%; position: relative; }	
	/* Select class */
	select { width:50%; padding:2px; background-color: #bbb;}
	/* Input type button, submit and reset */
	input[type=button], input[type=submit], input[type=reset] {
  		background-color: #4CAF50;
  		border: none;
  		color: white;
  		padding: 4px;
  		text-decoration: none;
  		margin: 2px;
  		cursor: pointer;
	}		
	/* Input ajax searching file */
	.search { padding:8px; background-color:lightblue; width:96%;}
	/* Ajax playlist file */
	#playlist { display:table; }
	#playlist li{ cursor:pointer; padding:1px; }
	#playlist li:hover{ color:blue; }
	/* Video playlist ajax */	
	#videoarea {
    	float:left;
    	width:98%;
		/* height:90%; */ 
    	margin:10px;    
	}			
    </style>

	<script src="assets/js/jquery-3.5.1.js"></script>
	<script>
	$(function() {
    	$("#playlist li").on("click", function() {
        	$("#videoarea").attr({
            	"src": $(this).attr("movieurl"),
            	"poster": "",
            	"autoplay": "autoplay"
        	})
    	})
    	$("#videoarea").attr({
        	"src": $("#playlist li").eq(0).attr("movieurl"),
        	"poster": $("#playlist li").eq(0).attr("moviesposter")
    	})
	
		$("#myInput").on("keyup", function() {
    		var value = $(this).val().toLowerCase();
    		$("#playlist li").filter(function() {
      			$(this).toggle($(this).text().toLowerCase().indexOf(value) > -1)
    		});
  		}); 


	})
	</script>
    
  </head>

  <body>

<div class="window" style="width: 100%">
  <div class="title-bar">
    <div class="title-bar-text">Video Player</div>
    <div class="title-bar-controls"><button aria-label="Help"></button></div>
  </div>
  <div class="window-body">


  <div class="row">

  <div class="colm_a">
  <fieldset><video id="videoarea" controls="controls" poster="" src=""></video></fieldset>
  </div>
  
  <div class="colm_b">
  
	<?php
	$pth = isset($_REQUEST["dirs"])? ($_REQUEST["dirs"]): "Asian" ;
	$url = "/~ants/media/videos/";
	$dsl = "/home/ants/public_html/media/videos/";
	$dir = $dsl."/".$pth;
	
	echo '<form action="" ><select name="dirs" width="80%"  onchange="this.form.submit()">';
	$sl = opendir($dsl);
	while (($dirsel = readdir($sl))!== false){
		if(is_dir($dirsel)!== true){ if($dirsel === $pth) {echo '<option value="'.$dirsel.'" selected>'.$dirsel.'</option>';}else{echo '<option value="'.$dirsel.'">'.$dirsel.'</option>';} }
	}
	echo '
	</select>&nbsp;<noscript><input type="submit" name="submit"></noscript>&nbsp;or back to</a>&nbsp;';
?>
      <button type="reset" onclick="location.href='.'">Home</button>
<?php
	echo '
	</form><br>
	<input type="text" class="search" id="myInput" onkeyup="myFunction()" placeholder="Search for names.." title="Type in a name"><br>
	
	<div id="rightcolumn">
	<div style="height:100%; font:12px/16px Georgia, Garamond, Serif;overflow:auto;">
	<ul id="playlist">
	
	';    
	
	$dh  = opendir($dir);
	while (($file = readdir($dh)) !== false) {
    	$match = preg_match("/.*\.(mp4|png|gif|jpeg|bmp)/", $file);
    	if ($match) { echo '<li movieurl="http://'.$_SERVER["SERVER_NAME"].$url.$pth.'/'.$file.'">'.$file.'</li>'; }
	}
	echo "</ul>";
	?>
	
  </div>
  </div><!-- row end -->

  </div><!-- windows-body -->
</div><!-- windows -->

  </body>
</html>
