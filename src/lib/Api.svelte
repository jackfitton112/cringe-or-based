<h1 class="text-white">Cringe or Based?</h1>
<hr/>
<script defer>
    // @ts-nocheck
    
    
    
        //TEST SCRIPT THAT PULLS 10  TITLES FROM THE API AND DISPLAYS THEM IN THE HTML FILE
    let i;
    let data = [];
    var index = 0;

    function getdata(subreddit){
    data.length = 0 
    document.getElementById("img").style = "height:20vh; width:auto;";
    document.getElementById("title").innerHTML = "Loading...";
    fetch(`https://www.reddit.com/r/${subreddit}/.json?limit=20`)
    .then(response => response.json())
    .then (x => {
        for (i = 0; i < 10; i++) {
            var title = x['data']['children'][i]['data']['title']
            var img = x['data']['children'][i]['data']['url']
            var text = x['data']['children'][i]['data']['selftext']
            if (img.match(/^https.*\.(jpeg|jpg|gif|png)$/) == null) {
                img = "https://www.redditstatic.com/icon.png"
            }
            if (text == null) {
                text = ""
            }
            var author = x['data']['children'][i]['data']['author']
            data.push({title, img, text, author})
        }}).then(() => {
            document.getElementById("title").innerHTML = data[0].title
            document.getElementById("img").src = data[0].img
        })

    }
    
    async function getmore(subreddit){
            var y = await fetch(`https://www.reddit.com/r/${subreddit}/.json?limit=100`)
            var x = await y.json()
            for (i = 10; i < 100; i++) {
            var title = x['data']['children'][i]['data']['title']
            var img = x['data']['children'][i]['data']['url']
            var text = x['data']['children'][i]['data']['selftext']
            if (img.match(/^https.*\.(jpeg|jpg|gif|png)$/) == null) {
                img = "https://www.redditstatic.com/icon.png"
            }
            if (text == null) {
                text = ""
            }
            var author = x['data']['children'][i]['data']['author']
            data.push({title, img, text, author})

    
    }

}

let subs = ["Select A Sub"]
fetch(`https://www.reddit.com/subreddits.json`)
.then(response => response.json())
.then(x => {
    for (i = 0; i < 24; i++) {
        var sub = x['data']['children'][i]['data']['display_name']
        subs.push(sub)
    }

    var select = document.getElementById("selecter");
    for (i = 0; i < subs.length; i++) {
        var opt = subs[i];
        var el = document.createElement("option");
        el.textContent = opt;
        el.value = opt;
        select.appendChild(el);
    }
})

    
    
    
    function next() {
        index++;
        if (index > data.length - 1) {
            index = 0;
        }
        document.getElementById("card1").style.backgroundColor = "";    
        document.getElementById("title").innerHTML = data[index].title;
        document.getElementById("img").src = data[index].img;
    }
    
    function cringe() {
        document.getElementById("title").innerHTML = "CRINGE";
        document.getElementById("card1").style.backgroundColor = "red";    
    }
    function based() {
        document.getElementById("title").innerHTML = "BASED";
        document.getElementById("card1").style.backgroundColor = "green";    
    }
    
    function prettycringe(){
        cringe();
        setTimeout(next, 500);
    }
    function prettybased(){
        based();
        setTimeout(next, 500);
       
    }

    function sel(){
        var x = document.getElementById("selecter");
        x.blur();
        getdata(x.value);
        getmore(x.value);
    }
    

    
    document.addEventListener(
      "keydown",
      function(event) {
        if(event.key == "ArrowRight") {
            prettybased();
        }
        else if(event.key == "ArrowLeft") {
            prettycringe();
        }
      },
    );

let touchstartX = 0
let touchendX = 0
let fingerCount = 0
    
const checkDirection = () => {

  const distance = 30 //Minimum distance for the swipe to work

  //left
  if (touchendX < touchstartX && (touchstartX - touchendX) > distance ){


  prettycringe()
   
  } 
  //right
  if (touchendX > touchstartX && (touchendX - touchstartX) > distance){

    prettybased()
        
    }
}

document.addEventListener('touchstart', e => {

    fingerCount = e.touches.length
    touchstartX = e.changedTouches[0].clientX  
     
})

document.addEventListener('touchend', e => {

    touchendX = e.changedTouches[0].clientX
    if(fingerCount === 1){ 
        checkDirection() 
    }
    
    
})
</script>
<div>
    
    <select on:change={sel} class="browser-default" id="selecter" ></select>

</div>
<!--
<div style="width:100%">

    <div id="card1">
        <center>
        <h1 id="title">Select A Sub</h1>
        <p id="selftext"></p>
        <p><img style="width:30%; height:auto;"alt="image from reddit" id="img"></p>
        <p id="auth"></p>
        </center>
    </div>

</div>
-->
<center>

<div style="max-height:50vh"class="row">
    <div class="col s12 m12">
      <div id="card1" class="card card1">
        <div class="card-image">
          <img style="display:none;" id="img" alt="from reddit">
        </div>
        <div class="card-content">
            <p id="title">Please select a subreddit from the list above</p>
        </div>
        </div>
      </div>
    </div>
    </center>
    <div class="card">

        <button name="button"class="waves-effect waves-light btn-large red"  on:click={prettycringe}>
            
            <slot>
            <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200" />
              Cringe
        
            </slot>
        </button>
      
        <button name="button" class="waves-effect waves-light btn-large green" on:click={prettybased}>
            
          <slot>
      
            Based
      
          </slot>
      </button>
      
      </div>
      <div class="footer">
        <p>Created by <a href="https://t2k-group.co.uk">T2K Group</a></p>
      </div>






