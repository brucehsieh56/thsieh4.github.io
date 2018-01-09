---
layout: default
---

# Hello!

Welcome to my website. I'm Bruce, I'd like to share my interesting experience in Machine Learning and Data Science, please have a look at my posts and works.

# Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.date | date: "%B %d, %Y" }}: {{ post.title }}</a>
    </li>
  {% endfor %}
</ul>


# Kaggle Kernels
- [Interactive Map of Pizza Restaurants in Python](https://www.kaggle.com/bbruce/interactive-map-of-pizza-restaurants-in-python)


# Notes
- [Python VS R command](/python_vs_R)


# Works

<div class="row">

  <div class="mb-4 col-xl-6">
    <div class="container" style="padding: 3px; border-radius: 4px; border: 1px solid #ddd; box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);">
      <div class="hovereffect" style="height: 18rem;">
        <img class="image mb-2" src="/figure/demo.png">
        <div class="middle">
          <a href="https://github.com/thsieh4/CSC522_project"><h4>Spotify Top 200 Tracks Prediction</h4></a>
          <p style="color:#f4f4f4; text-align:center;">A binary classifier capable of predicting whether a song will be a hit on Spotify.</p>
          <div class="progress mb-3">
            <div class="progress-bar progress-bar-striped progress-bar-animated bg-secondary" style="width:100%">100%</div>
          </div>
          <h6 class="fixed-bottom">
            <i class="material-icons md-16">local_offer</i>
            <span class="badge badge-primary">Team Project</span>
            <span class="badge badge-primary">Fall 2017</span>
            <span class="badge badge-secondary">Machine Learning</span>
            <span class="badge badge-secondary">Data Visualization</span>
            <span class="badge badge-success">Python</span>
            <span class="badge badge-info">scikit-learn</span>
            <span class="badge badge-info">pandas</span>
            <span class="badge badge-info">matplotlib</span>
            <span class="badge badge-info">seaborn</span>
          </h6>
        </div>
      </div>  
    </div>
  </div>
  
  <div class="mb-4 col-xl-6">
    <div class="container" style="padding: 3px; border-radius: 4px; border: 1px solid #ddd; box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);">
      <div class="hovereffect" style="height: 18rem;">
        <img class="image mb-2" src="/figure/Project_SingleViewMetrology.gif">
        <div class="middle">
          <h4>Single View Metrology with Line Segment Detector</h4>
          <p style="color:#f4f4f4; text-align:center;">Reconstructed the 3D box-shaped object from one single 2D image in terms of projective geometry.</p>
          <h6 class="fixed-bottom">
            <i class="material-icons md-16">local_offer</i>
            <span class="badge badge-primary">Team Project</span>
            <span class="badge badge-primary">Spring 2017</span>
            <span class="badge badge-secondary">Image Processing</span>
            <span class="badge badge-success">MATLAB</span>
            <span class="badge badge-success">Python</span>
            <span class="badge badge-info">OpenCV</span>
          </h6>
        </div>
      </div>
    </div>
  </div>

</div>


<div class="row">

  	<div class="mb-4 col-xl-6">
    	<div class="container" style="padding: 3px; border-radius: 4px; border: 1px solid #ddd; box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);">
      		<div class="hovereffect" style="height: 18rem;">
        		<img class="image mb-2" src="/figure/work_flood_detection.png">
        		<div class="middle">
          			<h4>Flood Detection</h4>
      				<li style="color:#f4f4f4; text-align:left;">Generated representative features by ultilizing a band-pass filter to flood / non-flood images.</li>
      				<li style="color:#f4f4f4; text-align:left;">Built flood detection models using SVM and Neural Network (F-score 0.88).</li>
         			<h6 class="fixed-bottom">
	        			<i class="material-icons md-16">local_offer</i>
			            <span class="badge badge-primary">Team Project</span>
			            <span class="badge badge-primary">Spring 2016</span>
			            <span class="badge badge-secondary">Feature Generation</span>
			            <span class="badge badge-secondary">Image Processing</span>
			            <span class="badge badge-secondary">Machine Learning</span>
			            <span class="badge badge-success">MATLAB</span>
          			</h6>
        		</div>
      		</div>  
    	</div>
    </div>

</div>