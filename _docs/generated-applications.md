---
layout: doc
title: "Adding Custom Code"
description: "Here is the description of the doc page"
date: 2018-11-08 8:14:30 +0600
post_image: assets/images/gep-high-level-components.jpg
category_name: Generated Applications
category_slug: the-apps
---

<b>Here we are going to see how you can add custom code to the generated code and deploy it to aws.</b>

<h5>Adding customer code instructions step by setp</h5>
<b>Create the front-end Application:</b>
<ul class="unorder-list">
    <li>create the project and go inside the project and click code generate.</li>
    <li>Once the code generated it will be pushed to the github.</li>
	<li>Clone the code from the github.</li>
	<li>Open terminal from the location where you have colned the code.<h5>$ cd applicationpath/application/clinets/desktop/projectName/src/app</h5></li>
    <li>Create a  Two new component 
    <ul class="unorder-list">
    <li> create-ticket Component </li>
    <li> ticket-details Component </li>
    </ul>
    <h5> ng generate component [component-name]</h5></li>
    <div> 
      <a><img src="{{site.baseurl}}/assets/images/component-generate.png" alt="Question Answer"></a>
    </div> 
    <li> Create the service for create-ticket component <h6>ng generate service [service-component-name]</h6></li>
     <a><img src="{{site.baseurl}}/assets/images/service-component.png" alt="Question Answer"></a>
     <p>Filename : <strong>create-ticket.service.ts</strong></p>Adding Custom Code
     <li>Components create the module </li>
     <h6>ng generate module [module-name]</h6>
     <a><img src="{{site.baseurl}}/assets/images/crete-ticket-module.png" alt="Question Answer"></a>
     <p>Filename : <strong>create-ticket.module.ts</strong>
      <a><img src="{{site.baseurl}}/assets/images/ticket-details-module.png" ></a>
     <p>Filename : <strong>create-ticket.module.ts</strong></p>
     <h6> Adding Custom Code</h6>
     <li>In the above are the screen shot of the components added and the html content.</li>
          <li>Once the you have added these you need to import the modules of the component to the app.module.ts file.</li>
               <li>So now you have added an custom component for the generated code</li>
      <a><img src="{{site.baseurl}}/assets/images/crete-ticket-component-html.png" ><a>
     <p>Filename : <strong>create-ticket.component.html</strong></p>
      <a><img src="{{site.baseurl}}/assets/images/create-ticket-module.png" ><a>
     <p>Filename : <strong>create-ticket.module.ts</strong></p>
      <a><img src="{{site.baseurl}}/assets/images/tciket-details-module.png" ><a>
     <p>Filename : <strong>ticket-details.module.ts</strong></p>
      <a><img src="{{site.baseurl}}/assets/images/appmodule.png" ><a>
     <p>Filename : <strong>app.module.ts</strong></p>
  <h5>Create the Backend Application:</h5>
    <h5>Prerequisites</h5>
  <pre><code class="language-html">
    <p> node -v 10.15.0</p>
    <p>npm -v 6.4.1 </p>
    <p>npm install -g typescript</p>
</code></pre>
<h5>Init your Package</h5>
<p>Let’s create a package.json file with all default values.</p>
<pre><code class="language-html">
    <p>npm init -y</p>
</code></pre>
  <div> 
      <a><img src="{{site.baseurl}}/assets/images/packagejson.png" alt="Question Answer"></a>
    </div>
    <ul class="order-list">
         <p><strong></strong></p>
    <p>Folder strcture</p>
    </ul>
    <a><img src="{{site.baseurl}}/assets/images/backendimages/folderStrcture.png" ><a>
     <p><strong></strong></p>
     <h6><p>Filename : <strong>Docker File</strong></p></h6>
     <a><img src="{{site.baseurl}}/assets/images/backendimages/docker.png" ><a>
      <p><strong></strong></p>
     <h6><p>Filename : <strong>server.ts</strong></p></h6>
     <a><img src="{{site.baseurl}}/assets/images/backendimages/serverts.png" ><a>
      <p><strong></strong></p>
     <h6><p>Filename : <strong>route.ts</strong></p></h6>
      <a><img src="{{site.baseurl}}/assets/images/backendimages/route.png" ><a>
      <p><strong></strong></p>
     <h6><p>Filename : <strong>controller.ts</strong></p></h6> 
     <a><img src="{{site.baseurl}}/assets/images/backendimages/controller.png" ><a>
     <p><strong></strong></p>
     <h6><p>Filename : <strong>service.ts</strong></p></h6> 
     <a><img src="{{site.baseurl}}/assets/images/backendimages/service.png" ><a>
     <p><strong></strong></p>
     <h6><p>Filename : <strong>dao.ts</strong></p></h6> 
     <a><img src="{{site.baseurl}}/assets/images/backendimages/dao.png" ><a>
     <p><strong></strong></p>
    <h6> <p>Filename : <strong>MongoConfig.ts</strong></p></h6> 
     <a><img src="{{site.baseurl}}/assets/images/backendimages/configmongo.png" ><a>
     <p><strong></strong></p>
     <h6><p>Filename : <strong>WistonLogger.ts</strong></p></h6> 
     <a><img src="{{site.baseurl}}/assets/images/backendimages/configwiston.png" ><a>
     <p><strong></strong></p>
     <h5>dependency package.json and runing scripts</h5>
    <h6><p>Filename : <strong>package.ts</strong></p></h6> 
     <a><img src="{{site.baseurl}}/assets/images/backendimages/finaljson.png" ><a>
    <p><strong></strong></p>
    <p><strong></strong></p>
     <h5>MicroService connect with ApiGateWay instructions below</h5>
     <h6>ApiGateway files</h6>
       <p><strong></strong></p>
     <h6><p>Filename : <strong>server.ts</strong></p></h6> 
     <a><img src="{{site.baseurl}}/assets/images/apiImages/apiserver.png" ><a>
      <p><strong></strong></p>
     <h6><p>Filename : <strong>Ticketcontroller.ts</strong></p></h6> 
     <a><img src="{{site.baseurl}}/assets/images/apiImages/apicontroller.png" ><a>
      <p><strong></strong></p>
     <h6><p>Filename : <strong>index.ts</strong></p></h6> 
     <a><img src="{{site.baseurl}}/assets/images/apiImages/apiIndex.png" ><a>
      <p><strong></strong></p>
     <h6><p>Filename : <strong>const.ts</strong></p></h6> 
     <a><img src="{{site.baseurl}}/assets/images/apiImages/apiconst.png" ><a>





<!-- 
<p>It’s no secret that the digital industry is booming. From exciting startups to global brandscomp nies are reaching out to digital agencies, responding to the new possibilities available. However, the industry is fast becoming overcrowded, heaving with agencies offering similar services — on the surface, </p>
 <h5>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</h5> -->

<!-- <pre><code class="language-html">.heading-1 {
<ul class="unorder-list">
    <li>Quam elementum pulvinar etiam non quam lacus suspendisse faucibus interdum posuere lorem ipsum</li>
    <li>Dolor sit amet consectetur adipiscing elit duis tristique </li>
    <li>Congue mauris rhoncus aenean vel </li>
</ul></code></pre>
<p>Filename : <strong>style.css</strong></p>Adding Custom Code
<h4 class="heading-4">Features 
</h4> -->
<!-- <ul class="unorder-list">
    <li>Quam elementum pulvinar etiam non quam lacus suspendisse faucibus interdum posuere lorem ipsum</li>
    <li>Dolor sit amet consectetur adipiscing elit duis tristique </li>
    <li>Congue mauris rhoncus aenean vel </li>
</ul> -->
<!-- <h4 class="heading-4">Credits</h4>
<ul class="order-list">
    <li>Bootstrap : css front-end framework. <a href="#">http://getbootstrap.com/</a></li>
    <li>Snazzy Maps :<a href="#">https://snazzymaps.com/</a></li>
    <li>jQuery :fast, small, and feature-rich JavaScript library <a href="#">http://jquery.com/</a></li>
    <li>Snazzy Maps :<a href="#">https://snazzymaps.com/</a></li>
    <li>Metismenu :<a href="#">https://github.com/onokumus/metismenu</a></li>
    <li>Highlight.js<a href="#">Syntax highlighting for the Web.https://highlightjs.org/</a></li>
</ul> -->
<!-- <h4 class="heading-4">Configure</h4>
<p>Being creative within the constraints of client briefs, budgets and timelines is the norm for most agencies. However, investing in research and development as a true, creative outlet is a powerful addition. In these side projects alone, your team members can pool their expertise to create and shape their own vision — a powerful way to develop motivation, interdisciplinary skills and close relationships. People think focus means
</p>
<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolorem dolores corporis recusandae ipsam iste ipsum quidem quisquam, a vel tenetur culpa, beatae officiis voluptatem atque voluptates eaque, commodi debitis, libero modi adipisci accusamus explicabo amet iusto! In culpa praesentium quos cupiditate deserunt quis, corrupti impedit tempore autem. Dolorem perspiciatis eius veritatis cum eaque magni.</p> -->
