<!-- slider from:  -->
<!-- https://github.com/mhayes/vue-twentytwenty -->
<head>
  <link rel="stylesheet" href="//unpkg.com/vue-twentytwenty/dist/vue-twentytwenty.css" />
</head> 
  <div id="app">
    <TwentyTwenty
      before="{{site.baseurl}}/img/mesh.jpg"
      beforeLabel="Mesh"
      after="{{site.baseurl}}/img/texture.jpg"
      afterLabel="Textured" />
  </div>
  3D models of Garcia Garden Quarry (mesh / textured)
  <script src="//unpkg.com/vue@2/dist/vue.js"></script>
  <script src="//unpkg.com/vue-twentytwenty/dist/vue-twentytwenty.js"></script>
  <script>
  new Vue({
    el: '#app'
  })
  </script>

&nbsp;
&nbsp;