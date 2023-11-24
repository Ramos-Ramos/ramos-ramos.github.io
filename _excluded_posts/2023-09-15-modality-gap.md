---
layout: distill
title: preliminary investigations into closing the modality gap
description: does incorporating non-contrastive learning objectives and weight sharing reduce the modality gap?
tags: distill formatting
giscus_comments: true
date: 2023-09-15
featured: true

authors:
  - name: Patrick Ramos
    url: "https://ramos-ramos.github.io/"
    # affiliations:
    #   name: ISLab, Osaka University
  - name: Ryan Ramos
    url: "https://en.wikipedia.org/wiki/Boris_Podolsky"
    # affiliations:
    #   name: ISLab, Osaka University

bibliography: 2023-09-15-modality-gap.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: A Recap of the Modality Gap
  - name: Ideas for Closing the Modality Gap
  - name: Another Metric for Measuring the Modality Gap
  - name: Preliminary Experiments
    subsections:
      - name: Sharing Weights
      - name: Adding Additional Learning Objectives
  - name: Closing Thoughts
  # - name: Equations
  #   # if a section has subsections, you can add them as follows:
  #   # subsections:
  #   #   - name: Example Child Subsection 1
  #   #   - name: Example Child Subsection 2
  # - name: Citations
  # - name: Footnotes
  # - name: Code Blocks
  # - name: Interactive Plots
  # - name: Layouts
  # - name: Other Typography?

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }

---

## A Recap of the Modality Gap

Multi-modal models, particularly those in the style of CLIP<d-cite key="radford2021learning"></d-cite>, encode images and texts into a joint semantic space with per-modality encoders trained with a contrastive learning objective. While this makes CLIP effective for zero-shot image classification, CLIP suffers from a phenomenon termed the *modality gap*<d-cite key="liang2022mind"></d-cite>, where image and text embeddings are far apart. Projecting CLIP embeddings to a two-dimensional space shows that text and image embeddings occupy different regions of latent space even when they should share semantic content.

<div class="l-page">
  <iframe src="{{ '/assets/blog_assets/2023-09-15-modality-gap/clip.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>
<div class="caption">
    We sample 500 image-text pairs from MS COCO<d-cite key="lin2014microsoft"></d-cite> and embed them with CLIP VIT-B/32. The embeddings are then projected to two-dimensional points with UMAP. Lines between points indicate image-text pairs.
</div>

There are two factors responsible for the modality gap:

1. The image and text encoders are randomly initialized separately. Because deep models like these encoders have narrow-cone-shaped embedding spaces, and different initializations create different latent spaces, having the separately initialized models creates the modality gap at the beginning of training. Proj

2. The contrastive loss function preserves the modality gap throughout traning.

Since closing the modality gap post-hoc can improve downstream performance<d-cite key="liang2022mind"></d-cite>, it may be of interest to train multi-modal models that don't have the gap to begin with.<d-footnote>It should noted however that this may come at the cost of fairness, hence it is not always desirable to want to close the modality gap.</d-footnote>

## Ideas for Closing the Modality Gap

If there are two major factors causing the modality gap, we can design interventions that target each one:

1. *The image and text encoders are randomly initialized separately.* What if the weights were tied?

2. *The contrastive loss function preserves the modality gap throughout traning.* What if a non-contrastive loss function was incorporated?

If we had the compute we could begin answering these questions by training 1) a baseline CLIP<d-footnote>We can't use the OpenAI's CLIP because it was trained on a private dataset. We would want all models in our experiments to be trained on the same data.</d-footnote>, 2) a CLIP with shared weights between the encoders, 3) a CLIP trained with an additional non-contrastive training objective, and 4) a CLIP with both the changes in 3 and 4. Then we would embed the same collection of images and texts with all models and compare the modality gap.

However, there is one problem: we don't have the compute. Instead, what we can offer is a preliminary investigation into these intervention using the closest available public models. These models include some degree of weight sharing or additional non-contrastive losses, however may also have different model hyperparameters (e.g. patch size), additional unrelated training components and different pretraining data. We provide a general overview of these models below and discuss them in more detail in later sections.

| Model | Weight Sharing | Loss | Training Data |
| --- | :---: | :---: | :---: |
| CLIP ViT-B/32 | ❌ | contrastive | private 400M-item dataset |
| MS-CLIP-S ViT-B/32| ✅* | contrastive | LAION-20M |
| BLIP ViT-B/16 | ❌ | contrastive, image-text matching, captioning | 14M-item dataset (MS COCO, Visual Genome, CC3M, CC12M, SBU Captoins)|
| CoCa | ❌ | contrastive, captioning | LAION-2B |


## Another Metric for Measuring the Modality Gap

The authors of the modality gap paper propose using the difference between the respective means of the image and text embeddings as a measure of the modality gap between a collection of image and text pairs. This works when running comparisons using one model, however this won't work if we want to compare the modality gap for different models. While one model can have a smaller difference between mean emebddings than another, that difference won't mean much if all of the image embeddings are still closer to each other than the text embeddings, and vice versa. What is more important is the distance of image and text embeddings to each other relative to other image and text embeddings respectively. We can therefore formulate a new modality gap metric as:

$$
\text{similarity ratio} = \frac{\frac{1}{\vert I \vert} \sum_{i=0}^{\vert I \vert - 1}\cos(I_i, T_i)}{\frac{1}{\vert I \vert + \vert T \vert} (\sum_{i \lt j}\cos(I_i, I_j) + \sum_{i \lt j}\cos(T_i, T_j))}
$$

where $$I$$ are $$l_2$$-normalized image embeddings, $$T$$ are $$l_2$$-normalized text embeddings, and $$I_i$$ and $$T_i$$ form a matching image text pair. This metric, which we term *similarity ratio*, measure the modality gap as a ratio of the average cosine similarity of corresponding images and texts to the average cosine similarity of images and texts to all other points in their respective modalities. The the average inter-modality similarity is larger than the average intra-modality similarity, meaning the similarity ratio is larger and above 1, then the modality gap is smaller.

While this metric is capable of measuring the gap between image and text embeddings, one caveat is that it is not sensitive to which texts images are close to or vice versa. For example, it is possible for an image to be closer to its corresponding text than to other images, but for that  image to be even closer to the wrong text than the right one. However, since our goal is simply to measure the modality gap and not necessarily the correctness of image-text similarities, accounting for the latter is outside the scope of these exerpiments.

<!-- Another approach, albeit incomplete, to measuring the modality gap is based on the idea that in observations of the modality gap, image and text embeddings tend to form clusters. This means we can assume all image and text embeddings belong to their own clusters and take the silhouette score of the points. If the modality gap is present, this method of assigning clusters should yield a high similarity score. The caveat of this metric however is that while a low silhouette score indicates the lack of a modality gap, it does not indicate that corresponding image and text embeddings are close to each other. It is possible for all image and text embeddings to be close to each other, but to the wrong embeddings (i.e. dog images are close to cat captions). -->


## Preliminary Experiments

### Sharing Weights

*Note: The experiment in this section is quite meaningless*

For a model representative of weight sharing, we use MS-CLIP-S<d-cite key="you2022learning"></d-cite>. MS-CLIP ties the weights for all corresponding CLIP image and text encoder layers except for the input embedding, layer normalizations, and output projectors. While this is not complete weight tying, the authors if MS-CLIP show that exlcuding these layers maximizes performance under a weight-sharing setting. Another caveat is that the only available MS-CLIP models are those of the MS-CLIP-S architecture, which incorporates additional components not related to weight tying, specifically modality-specific encoders prepended to the tied weight encoders and a parallel CNN stream to compliment the image CLIP encoder.

Projecting 500 image-text pairs embedded with MS-CLIP-S shows that the modality gap still exists. Furthermore, while the similarity ratio for 5000 image-text pairs is higher with MS-CLIP-S than CLIP, the ratio is still below 1, indicating that embeddings of one modality are still closer to each other than embeddings of the other modality that share semantic content. Unfortunately, this experiment was quite futile from the get-go. Whether or not there was a change in the modality gap, there's no saying whether it could be attributable to MS-CLIP-S because of the specialized components.

| Model | Similarity Ratio|
| --- | :---: |
| CLIP | 0.62 |
| MS-CLIP-S | 0.79 |

<div class="l-page">
  <iframe src="{{ '/assets/blog_assets/2023-09-15-modality-gap/weight_sharing.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>
<!-- <div class="caption">
    We sample 500 image-text pairs from MS COCO<d-cite key="lin2014microsoft"></d-cite> and embed them with CLIP VIT-B/32. The embeddings are then projected to two-dimensional points with UMAP. Lines between points indicate image-text pairs.
</div> -->

### Adding Additional Learning Objectives

We explore two models for additionative, non-contrastive learning objectives: BLIP and CoCa. Both models use contrastive learning, however BLIP adds an image-text matching loss and a captioning loss while CoCa adds just a captioning loss.

Even when adding these additional objectives, contrastive learning still appears to maintain the modality gap created at random initialization.

<div class="l-page">
  <iframe src="{{ '/assets/blog_assets/2023-09-15-modality-gap/objectives.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>
<!-- <div class="caption">
    We sample 500 image-text pairs from MS COCO<d-cite key="lin2014microsoft"></d-cite> and embed them with CLIP VIT-B/32. The embeddings are then projected to two-dimensional points with UMAP. Lines between points indicate image-text pairs.
</div> -->

| Model | Similarity Ratio|
| --- | :---: |
| CLIP | 0.62 |
| BLIP | 0.47 |
| CoCa | 0.82 |

## Closing Thoughts

These results are very far from conclusive. MS-CLIP does not share all weights and incorporates architectural components not present in CLIP, while BLIP and CoCa are not exhaustive of training objectives that could counteract contrastive learning. As a result, we shouldn't take these results to immediately mean that the modality gap persists with weight sharing and additional non-contrastive training losses.

<!-- ## Equations

This theme supports rendering beautiful math in inline and display modes using [MathJax 3](https://www.mathjax.org/) engine.
You just need to surround your math expression with `$$`, like `$$ E = mc^2 $$`.
If you leave it inside a paragraph, it will produce an inline expression, just like $$ E = mc^2 $$.

To use display mode, again surround your expression with `$$` and place it as a separate paragraph.
Here is an example:

$$
\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
$$

Note that MathJax 3 is [a major re-write of MathJax](https://docs.mathjax.org/en/latest/upgrading/whats-new-3.0.html) that brought a significant improvement to the loading and rendering speed, which is now [on par with KaTeX](http://www.intmath.com/cg5/katex-mathjax-comparison.php).

***

## Citations

Citations are then used in the article body with the `<d-cite>` tag.
The key attribute is a reference to the id provided in the bibliography.
The key attribute can take multiple ids, separated by commas.

The citation is presented inline like this: <d-cite key="gregor2015draw"></d-cite> (a number that displays more information on hover).
If you have an appendix, a bibliography is automatically created and populated in it.

Distill chose a numerical inline citation style to improve readability of citation dense articles and because many of the benefits of longer citations are obviated by displaying more information on hover.
However, we consider it good style to mention author last names if you discuss something at length and it fits into the flow well — the authors are human and it’s nice for them to have the community associate them with their work.

***

## Footnotes

Just wrap the text you would like to show up in a footnote in a `<d-footnote>` tag.
The number of the footnote will be automatically generated.<d-footnote>This will become a hoverable footnote.</d-footnote>

***

## Code Blocks

Syntax highlighting is provided within `<d-code>` tags.
An example of inline code snippets: `<d-code language="html">let x = 10;</d-code>`.
For larger blocks of code, add a `block` attribute:

<d-code block language="javascript">
  var x = 25;
  function(x) {
    return x * x;
  }
</d-code>

**Note:** `<d-code>` blocks do not look good in the dark mode.
You can always use the default code-highlight using the `highlight` liquid tag:

{% highlight javascript %}
var x = 25;
function(x) {
  return x * x;
}
{% endhighlight %}

***

## Interactive Plots

You can add interative plots using plotly + iframes :framed_picture:

<div class="l-page">
  <iframe src="{{ '/assets/plotly/demo.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>

The plot must be generated separately and saved into an HTML file.
To generate the plot that you see above, you can use the following code snippet:

{% highlight python %}
import pandas as pd
import plotly.express as px
df = pd.read_csv(
  'https://raw.githubusercontent.com/plotly/datasets/master/earthquakes-23k.csv'
)
fig = px.density_mapbox(
  df,
  lat='Latitude',
  lon='Longitude',
  z='Magnitude',
  radius=10,
  center=dict(lat=0, lon=180),
  zoom=0,
  mapbox_style="stamen-terrain",
)
fig.show()
fig.write_html('assets/plotly/demo.html')
{% endhighlight %}

***

## Details boxes

Details boxes are collapsible boxes which hide additional information from the user. They can be added with the `details` liquid tag:

{% details Click here to know more %}
Additional details, where math $$ 2x - 1 $$ and `code` is rendered correctly.
{% enddetails %}

***

## Layouts

The main text column is referred to as the body.
It is the assumed layout of any direct descendants of the `d-article` element.

<div class="fake-img l-body">
  <p>.l-body</p>
</div>

For images you want to display a little larger, try `.l-page`:

<div class="fake-img l-page">
  <p>.l-page</p>
</div>

All of these have an outset variant if you want to poke out from the body text a little bit.
For instance:

<div class="fake-img l-body-outset">
  <p>.l-body-outset</p>
</div>

<div class="fake-img l-page-outset">
  <p>.l-page-outset</p>
</div>

Occasionally you’ll want to use the full browser width.
For this, use `.l-screen`.
You can also inset the element a little from the edge of the browser by using the inset variant.

<div class="fake-img l-screen">
  <p>.l-screen</p>
</div>
<div class="fake-img l-screen-inset">
  <p>.l-screen-inset</p>
</div>

The final layout is for marginalia, asides, and footnotes.
It does not interrupt the normal flow of `.l-body` sized text except on mobile screen sizes.

<div class="fake-img l-gutter">
  <p>.l-gutter</p>
</div>

***

## Other Typography?

Emphasis, aka italics, with *asterisks* (`*asterisks*`) or _underscores_ (`_underscores_`).

Strong emphasis, aka bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~

1. First ordered list item
2. Another item
⋅⋅* Unordered sub-list.
1. Actual numbers don't matter, just that it's a number
⋅⋅1. Ordered sub-list
4. And another item.

⋅⋅⋅You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

⋅⋅⋅To have a line break without a paragraph, you will need to use two trailing spaces.⋅⋅
⋅⋅⋅Note that this line is separate, but within the same paragraph.⋅⋅
⋅⋅⋅(This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)

* Unordered list can use asterisks
- Or minuses
+ Or pluses

[I'm an inline-style link](https://www.google.com)

[I'm an inline-style link with title](https://www.google.com "Google's Homepage")

[I'm a reference-style link][Arbitrary case-insensitive reference text]

[I'm a relative reference to a repository file](../blob/master/LICENSE)

[You can use numbers for reference-style link definitions][1]

Or leave it empty and use the [link text itself].

URLs and URLs in angle brackets will automatically get turned into links.
http://www.example.com or <http://www.example.com> and sometimes
example.com (but not on Github, for example).

Some text to show that the reference links can follow later.

[arbitrary case-insensitive reference text]: https://www.mozilla.org
[1]: http://slashdot.org
[link text itself]: http://www.reddit.com

Here's our logo (hover to see the title text):

Inline-style:
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

Reference-style:
![alt text][logo]

[logo]: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 2"

Inline `code` has `back-ticks around` it.

```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```

```python
s = "Python syntax highlighting"
print s
```

```
No language indicated, so no syntax highlighting.
But let's throw in a <b>tag</b>.
```

Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the
raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.


Here's a line for us to start with.

This line is separated from the one above by two newlines, so it will be a *separate paragraph*.

This line is also a separate paragraph, but...
This line is only separated by a single newline, so it's a separate line in the *same paragraph*. -->
