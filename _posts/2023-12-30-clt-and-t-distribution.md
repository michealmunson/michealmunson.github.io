---
layout: post
title:  "A biologist's dream - the T distribution and its hypothesis tests"
categories: Statistics
---
<script>
MathJax = {
  tex: {
    inlineMath: [['$', '$'], ['\\(', '\\)']]
  }
};
</script>
<script id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
</script>

If you've ever worked in a biologist's lab, or even read an occasional paper in cell biology, you'll notice that the sample sizes collected are relatively low compared to other fields. This is often the case because of how inaccessible biological or medical data is. Data collection in a lot of fields is highly accessible - collection procedures include things like surveying or collecting images of people doing common activities. In a biological or medical setting, it can be quite expensive and even risky to collect even one data point. 

Consider a researcher interested in studying solid tumor physiology. That poor soul needs to get a solid tumor to study somehow. One approach is actually finding a patient willing to donate a piece of their tumor. Another is artificially generating one in a lab. Without going into details into either approach, I'm sure you can already imagine the costs and risks associated with collecting such information.

Hence, the T distribution.

