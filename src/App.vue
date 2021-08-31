<template>
  <div id="app">
    <div class="wrapper">
      <div class="container">
        <router-view/>
      </div>
    </div>
  </div>
</template>

<style lang="scss">

//importing font for all our application
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');

//null styles
*{
  padding: 0;
  margin: 0;
  border: 0;
}

*,*:before,*:after{
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}

:focus,:active{outline:none;}
a:focus,a:active{outline: none;}

nav,footer,header,aside{display: block;}

html,body{
  height: 100%;
  width: 100%;
  font-size: 100%;
  line-height: 1;
  font-size: 14px;
  -ms-text-size-adjust: 100%;
  -moz-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}

input,button,textarea{font-family: inherit;}

input::-ms-clear{display: none;}
button{cursor: pointer;}
button::-moz-focus-inner{padding: 0;border: 0;}
a,a:visited{text-decoration: none;}
a:hover{text-decoration: none;}
ul li{list-style: none;}
img{vertical-align: top;}


// container and wrapper styles
$maxWidthContainer: 1180px;
$paddingContainer: 0px 15px;
.container{
  max-width: $maxWidthContainer;
  padding: $paddingContainer;
  margin: 0 auto;
}

.wrapper{
  overflow: hidden;
  min-height: 100%;
  width: 100%;
}

// main application styles
#app {
  min-height: 100vh;
  padding-top: 25px;
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #eee;
  background: lightslategray;
}
</style>
