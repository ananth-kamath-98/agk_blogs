---
layout: post
title:  "Too Lazy To Change My Wallpaper."
description: I wrote a python script that automatically changes my desktop wallpaper everyday. 
date:   2020-05-09
categories: Automation
---
<p><b>Me on a monday morning:</b> <i>*log's in*</i>
<br/><b>Also me:</b> Ugh! I hate this wallpaper!</p>
<p> Okay, I admit that i'm being too dramatic about this. Although, it isn't really that far from true. I am notorious for constantly getting bored of my wallpaper every few days. Yes, I am one of those people who has to constantly change their wallpaper just to get the day going. My go-to source for wallpapers is <a href="https://unsplash.com/" target="_blank">Unsplash</a>. I love images that are vibrant and have a lot of minute details. Very original, I know!</p>
<p> While I was on my last wallpaper hunt, I thought of an idea to automate this. By scheduling a change of wallpaper to happen every now and then, I could avoid the otherwise annoying task of hunting for a new one and then setting it. My mom would be so disappointed if she knew i'd become this lazy.<br/> Moving on...here's what I did!</p>

<!-- Section One -->
<h2><b> Where do I download the wallpapers from? </b></h2>
<p> Unsplash provides access to their API to anyone who creates an account.
<br/>I visited their developers site and registered a new application.
<br/> In short, this is what I did:<br/>
<ol>
    <li>Visit:  <a href="https://unsplash.com/developers"> Unsplash Developers</a> and signup.</li>
    <li>Click on <b>Your Apps->New Application</b>.</li>
    <li>Be responsible and blindly accept all the terms and conditions like you always do. </li>
    <li>Put in your Application name and a small description.</li>
    <li>You can apply for production if that's what you are going for. But I would read the Terms and conditions once again before I were you. Otherwise, you might lose access to the API.</li>
    <li>Scroll down to the <b>Access Key</b> and copy it. We'll be needing it for authorisation with the API.</li>
</ol>
</p>

<!-- Section Two-->
<h2><b>Let's Code!</b></h2>
<p><b>Step 1:</b> Import all the libraries.</p>

<script src="https://gist.github.com/agk98/602f4f9bdb348601301cec8bd0a1ca9f.js"></script>

<ul>
    <li> The <b>requests</b> library helps with issuing <b>GET</b> requests to the Unsplash API. <b>GET</b> requests are used to retrieve data. Which is what we are doing here.</li>
    <li> The <b>urllib.request</b> library allows us to download the image once we have it's URL.</li>
    <li> The <b>ctypes</b> library allows us to interact with windows using python. This gives us the function which allows the script to change the desktop background.</li>
    <li> The <b>random</b> library is used to spit out random numbers. </li>
    <li> The <b>datetime</b> library gives us functions to retrieve the time and date details.</li>
</ul> 

<p><b>Step 2:</b> Download the wallpaper.</p>

<script src="https://gist.github.com/agk98/2721c8ad483b420a002092efb0efd5f5.js"></script>

<ul>
    <li> <b>filters</b> is a list of all the filters which I liked. The code would pick one filter at random and download an image of that filter. This reduced the chances of me getting bored of just one filter.</li>
    <li> <b>per_page</b> is the number of search results I want to be retrieved. With 20 results, it reduces the chances of the same image being retrieved when the a filter is applied more than once.</li>
    <li> <b>client_id</b> is the access key which I'd got when i registered the application on the unsplash api website. This code is for authorisation between my computer with the API when I am querying it for images. </li>
</ul>

<script src="https://gist.github.com/agk98/7a2cde11c2fb79799d8bb1fc90c77b3c.js"></script>

<ul>
    <li> <b>query_string</b> is the query that is sent to the API. The string is formatted with the filter which is chosen at random by the <b>random.choice</b> function, the number of search results and the access key.</li>
    <li> <b>requests.get</b> function is used to issue a <b>GET</b> request to the API. The result is returned to <b>get_request</b> as a response object.</li>
    <li> <b>data</b> is assigned the json data which is retrieved from <b>get_request</b> using the <b>.json()</b> function.</li>
</ul>

<p> The structure of the json data in <b>data</b> can be seen as:</p>
<div style="text-align:center">
    <img src="/images/blog_2_image_data.png">
</div>
<p> The search results can be accessed with the <b>'results'</b> key of the json data. As set, there are 20 results. Inside each of the 20 results, there are various fields. These fields contain the respective details of that respective image. The attribute that we are interested is in the <b>'urls'</b> attribute. 
As we are using the image as a desktop background, I prefer downloading the highest quality image which is in the raw format. The URL to this raw image is accessed with <b> data['results'][0]['urls']['raw']</b>. If you visit the link, you will see that it displays the full image in its highest resolution.

<script src="https://gist.github.com/agk98/3b01cc00064adee90b187ab8639de19f.js"></script>

<ul>
    <li> <b>file_name</b> stores the name of the file it will be saved with on the local machine. This is set as the date when it was downloaded. This would allow for easy search incase I want to use it for another purpose.</li>
    <li> The unsplash API allowing for dynamic resizing of the image. So, i can set a certain number of parameters which will be applied to the image, ensuring that I get the image in the specification that I want. The <b>dynamic_resize</b> variable stores the suffix which will be added to the image url and stored in <b>retrieval_url</b> before the image is downloaded.<br/>
    A complete list of the possible parameters can be found at <a href="https://unsplash.com/documentation">API Documentation</a>.</li>
    <li> <b>url.urlretrieve(retrieval_url, file_name)</b> downloads the image using the url and stores it in the current working directory with the file_name. </b></li>
</ul>

<p><b>Step 3:</b> Setting the wallpaper</p>
<script src="https://gist.github.com/agk98/d57b06059621c18fa926613e28a68648.js"></script>
<ul>
    <li> The <b>SystemParametersInfoW</b> function is a direct Windows interface.</li>
    <li> The <b>SPI_SETDESKTOPWALLPAPER</b> is named as such because this is how it will be referenced in all of windows documentation.</li>
    <li> The <b>pathToImage</b> has the full path to the image which is required for windows to access it.</li>
    <li> The rest of the parameters are certain flags which can be used for resizing and other functionalities.</li>
</ul>

<p> This script when run, can now change the background of my Windows PC.</p>

<!-- Section Three -->
<h2><b>How do I get complete automation?</b></h2>
<p> Now what remained was for me to schedule this script to run everyday. This way, it would change the wallpaper of my desktop everyday and I wont have to go hunting for a new one constantly. I did this using the native Windows Task Scheduler. With this, I set my script to run everyday at 09:30AM.</p>
<p> Here's a tutorial if you need help with using the Windows Task Scheduler. </p>
<h4><u><b><a href="https://www.youtube.com/watch?v=n2Cr_YRQk7o" target="_blank"> Schedule Python Scripts with Windows Task Scheduler</a></b></u></h4>

<h2><b> Thank You!</b></h2>
<p> You have made it to the end of this article. This is how I took care of the menacing task of searching for a new wallpaper every few days.<p>

<p> All my code for this project is in my GitHub Repositoy.</p>
<div style="text-align:center">
    <a class="no_underline" href="https://github.com/agk98/auto_wallpaper" target="_blank"><input type="button" value="AutoWallpaper's Source Code"></a>
</div>



<br/> 
I hope this helps you too!<br/>
Happy Coding!</p> 
