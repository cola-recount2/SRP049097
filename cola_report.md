cola Report for Consensus Partitioning
==================

**Date**: 2019-12-05 03:34:51 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 17713    54
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[CV:hclust](#CV-hclust)     |          3| 1.000|           0.972|       0.988|** |2          |
|[CV:NMF](#CV-NMF)           |          3| 1.000|           0.957|       0.981|** |           |
|[MAD:pam](#MAD-pam)         |          3| 1.000|           0.974|       0.980|** |2          |
|[ATC:skmeans](#ATC-skmeans) |          2| 1.000|           1.000|       1.000|** |           |
|[SD:mclust](#SD-mclust)     |          3| 0.969|           0.942|       0.973|** |           |
|[SD:hclust](#SD-hclust)     |          4| 0.964|           0.970|       0.977|** |2,3        |
|[SD:skmeans](#SD-skmeans)   |          3| 0.958|           0.917|       0.963|** |           |
|[MAD:skmeans](#MAD-skmeans) |          3| 0.952|           0.958|       0.980|** |2          |
|[MAD:hclust](#MAD-hclust)   |          6| 0.946|           0.940|       0.967|*  |4          |
|[ATC:pam](#ATC-pam)         |          3| 0.944|           0.940|       0.973|*  |           |
|[CV:skmeans](#CV-skmeans)   |          2| 0.925|           0.947|       0.978|*  |           |
|[ATC:kmeans](#ATC-kmeans)   |          2| 0.925|           0.925|       0.973|*  |           |
|[SD:NMF](#SD-NMF)           |          6| 0.916|           0.848|       0.902|*  |2,3,5      |
|[SD:pam](#SD-pam)           |          4| 0.826|           0.876|       0.914|   |           |
|[MAD:mclust](#MAD-mclust)   |          3| 0.759|           0.901|       0.890|   |           |
|[MAD:NMF](#MAD-NMF)         |          2| 0.753|           0.914|       0.955|   |           |
|[CV:mclust](#CV-mclust)     |          2| 0.632|           0.909|       0.941|   |           |
|[ATC:hclust](#ATC-hclust)   |          2| 0.602|           0.703|       0.891|   |           |
|[ATC:NMF](#ATC-NMF)         |          3| 0.592|           0.781|       0.884|   |           |
|[CV:pam](#CV-pam)           |          4| 0.481|           0.752|       0.855|   |           |
|[SD:kmeans](#SD-kmeans)     |          3| 0.382|           0.817|       0.814|   |           |
|[ATC:mclust](#ATC-mclust)   |          2| 0.377|           0.685|       0.872|   |           |
|[MAD:kmeans](#MAD-kmeans)   |          3| 0.255|           0.726|       0.764|   |           |
|[CV:kmeans](#CV-kmeans)     |          3| 0.229|           0.669|       0.759|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 1.000           0.974       0.989         0.4691 0.525   0.525
#&gt; CV:NMF      2 0.852           0.931       0.971         0.4357 0.547   0.547
#&gt; MAD:NMF     2 0.753           0.914       0.955         0.4989 0.497   0.497
#&gt; ATC:NMF     2 0.813           0.914       0.962         0.4481 0.547   0.547
#&gt; SD:skmeans  2 0.683           0.948       0.968         0.4942 0.508   0.508
#&gt; CV:skmeans  2 0.925           0.947       0.978         0.4934 0.508   0.508
#&gt; MAD:skmeans 2 1.000           0.992       0.996         0.5025 0.497   0.497
#&gt; ATC:skmeans 2 1.000           1.000       1.000         0.5036 0.497   0.497
#&gt; SD:mclust   2 0.625           0.888       0.943         0.4726 0.525   0.525
#&gt; CV:mclust   2 0.632           0.909       0.941         0.4795 0.508   0.508
#&gt; MAD:mclust  2 0.755           0.917       0.939         0.4337 0.525   0.525
#&gt; ATC:mclust  2 0.377           0.685       0.872         0.4834 0.508   0.508
#&gt; SD:kmeans   2 0.249           0.703       0.814         0.4376 0.525   0.525
#&gt; CV:kmeans   2 0.620           0.849       0.912         0.2706 0.743   0.743
#&gt; MAD:kmeans  2 0.201           0.630       0.761         0.4427 0.508   0.508
#&gt; ATC:kmeans  2 0.925           0.925       0.973         0.4930 0.508   0.508
#&gt; SD:pam      2 0.478           0.895       0.897         0.4250 0.575   0.575
#&gt; CV:pam      2 0.665           0.904       0.947         0.2366 0.743   0.743
#&gt; MAD:pam     2 0.922           0.922       0.968         0.4748 0.525   0.525
#&gt; ATC:pam     2 0.673           0.879       0.944         0.4820 0.497   0.497
#&gt; SD:hclust   2 1.000           0.972       0.980         0.4005 0.609   0.609
#&gt; CV:hclust   2 1.000           1.000       1.000         0.0736 0.927   0.927
#&gt; MAD:hclust  2 0.852           0.947       0.961         0.4819 0.497   0.497
#&gt; ATC:hclust  2 0.602           0.703       0.891         0.4446 0.525   0.525
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 0.973           0.965       0.983          0.397 0.793   0.616
#&gt; CV:NMF      3 1.000           0.957       0.981          0.507 0.726   0.528
#&gt; MAD:NMF     3 0.807           0.878       0.949          0.314 0.709   0.487
#&gt; ATC:NMF     3 0.592           0.781       0.884          0.469 0.732   0.529
#&gt; SD:skmeans  3 0.958           0.917       0.963          0.349 0.768   0.567
#&gt; CV:skmeans  3 0.771           0.890       0.932          0.344 0.765   0.566
#&gt; MAD:skmeans 3 0.952           0.958       0.980          0.327 0.723   0.499
#&gt; ATC:skmeans 3 0.759           0.870       0.934          0.322 0.760   0.547
#&gt; SD:mclust   3 0.969           0.942       0.973          0.369 0.832   0.680
#&gt; CV:mclust   3 0.514           0.747       0.829          0.284 0.832   0.670
#&gt; MAD:mclust  3 0.759           0.901       0.890          0.478 0.832   0.680
#&gt; ATC:mclust  3 0.659           0.800       0.868          0.327 0.712   0.489
#&gt; SD:kmeans   3 0.382           0.817       0.814          0.382 0.760   0.565
#&gt; CV:kmeans   3 0.229           0.669       0.759          0.711 0.860   0.817
#&gt; MAD:kmeans  3 0.255           0.726       0.764          0.380 0.804   0.625
#&gt; ATC:kmeans  3 0.477           0.700       0.796          0.301 0.804   0.625
#&gt; SD:pam      3 0.719           0.831       0.885          0.441 0.782   0.621
#&gt; CV:pam      3 0.746           0.866       0.948          0.334 0.992   0.989
#&gt; MAD:pam     3 1.000           0.974       0.980          0.365 0.793   0.616
#&gt; ATC:pam     3 0.944           0.940       0.973          0.296 0.709   0.507
#&gt; SD:hclust   3 0.913           0.949       0.965          0.340 0.857   0.766
#&gt; CV:hclust   3 1.000           0.972       0.988          2.076 0.866   0.855
#&gt; MAD:hclust  3 0.882           0.932       0.970          0.162 0.925   0.848
#&gt; ATC:hclust  3 0.517           0.487       0.712          0.342 0.883   0.776
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.853           0.824       0.812         0.1117 0.866   0.627
#&gt; CV:NMF      4 0.799           0.744       0.823         0.0899 0.962   0.887
#&gt; MAD:NMF     4 0.777           0.819       0.874         0.1051 0.944   0.840
#&gt; ATC:NMF     4 0.573           0.636       0.797         0.1105 0.896   0.695
#&gt; SD:skmeans  4 0.855           0.930       0.942         0.1337 0.877   0.647
#&gt; CV:skmeans  4 0.698           0.820       0.838         0.1323 0.902   0.713
#&gt; MAD:skmeans 4 0.801           0.812       0.881         0.1280 0.880   0.653
#&gt; ATC:skmeans 4 0.753           0.677       0.791         0.0944 0.927   0.781
#&gt; SD:mclust   4 0.827           0.796       0.894         0.1249 0.871   0.653
#&gt; CV:mclust   4 0.671           0.821       0.893         0.1060 0.894   0.720
#&gt; MAD:mclust  4 0.716           0.820       0.900         0.1241 0.925   0.789
#&gt; ATC:mclust  4 0.449           0.449       0.647         0.1088 0.939   0.826
#&gt; SD:kmeans   4 0.502           0.723       0.789         0.1504 0.983   0.951
#&gt; CV:kmeans   4 0.275           0.470       0.653         0.3073 0.681   0.507
#&gt; MAD:kmeans  4 0.496           0.727       0.770         0.1546 1.000   1.000
#&gt; ATC:kmeans  4 0.496           0.602       0.699         0.1027 0.855   0.643
#&gt; SD:pam      4 0.826           0.876       0.914         0.1369 0.966   0.906
#&gt; CV:pam      4 0.481           0.752       0.855         0.4810 0.871   0.826
#&gt; MAD:pam     4 0.824           0.739       0.888         0.1034 0.978   0.935
#&gt; ATC:pam     4 0.890           0.896       0.948         0.0673 0.961   0.900
#&gt; SD:hclust   4 0.964           0.970       0.977         0.3184 0.816   0.604
#&gt; CV:hclust   4 0.665           0.661       0.886         0.7175 0.877   0.845
#&gt; MAD:hclust  4 0.913           0.934       0.970         0.2629 0.860   0.668
#&gt; ATC:hclust  4 0.638           0.650       0.857         0.1230 0.888   0.731
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.946           0.958       0.954         0.0833 0.964   0.850
#&gt; CV:NMF      5 0.749           0.792       0.786         0.0919 0.867   0.575
#&gt; MAD:NMF     5 0.895           0.881       0.919         0.0830 0.916   0.716
#&gt; ATC:NMF     5 0.545           0.560       0.748         0.0591 0.808   0.439
#&gt; SD:skmeans  5 0.879           0.903       0.929         0.0639 0.939   0.755
#&gt; CV:skmeans  5 0.834           0.739       0.867         0.0755 0.927   0.717
#&gt; MAD:skmeans 5 0.816           0.792       0.870         0.0708 0.941   0.763
#&gt; ATC:skmeans 5 0.677           0.638       0.761         0.0596 0.925   0.740
#&gt; SD:mclust   5 0.864           0.815       0.859         0.0450 0.941   0.794
#&gt; CV:mclust   5 0.695           0.847       0.871         0.0647 0.925   0.763
#&gt; MAD:mclust  5 0.641           0.617       0.749         0.0855 0.897   0.650
#&gt; ATC:mclust  5 0.560           0.505       0.642         0.0936 0.883   0.661
#&gt; SD:kmeans   5 0.647           0.490       0.722         0.0849 0.913   0.739
#&gt; CV:kmeans   5 0.318           0.372       0.630         0.1283 0.762   0.427
#&gt; MAD:kmeans  5 0.590           0.310       0.664         0.0766 0.927   0.786
#&gt; ATC:kmeans  5 0.496           0.411       0.625         0.0723 0.983   0.949
#&gt; SD:pam      5 0.798           0.832       0.880         0.0870 0.916   0.741
#&gt; CV:pam      5 0.652           0.681       0.811         0.3212 0.776   0.638
#&gt; MAD:pam     5 0.885           0.773       0.905         0.0883 0.925   0.767
#&gt; ATC:pam     5 0.662           0.794       0.840         0.0723 0.972   0.920
#&gt; SD:hclust   5 0.852           0.927       0.902         0.0583 0.966   0.881
#&gt; CV:hclust   5 0.716           0.872       0.928         0.2971 0.776   0.672
#&gt; MAD:hclust  5 0.882           0.861       0.900         0.0473 0.994   0.980
#&gt; ATC:hclust  5 0.614           0.762       0.784         0.1119 0.857   0.588
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.916           0.848       0.902         0.0468 0.944   0.743
#&gt; CV:NMF      6 0.893           0.750       0.854         0.0575 0.952   0.766
#&gt; MAD:NMF     6 0.851           0.791       0.854         0.0494 0.950   0.780
#&gt; ATC:NMF     6 0.656           0.576       0.750         0.0521 0.881   0.556
#&gt; SD:skmeans  6 0.867           0.840       0.868         0.0352 1.000   1.000
#&gt; CV:skmeans  6 0.795           0.671       0.804         0.0361 0.947   0.752
#&gt; MAD:skmeans 6 0.819           0.733       0.817         0.0376 0.952   0.766
#&gt; ATC:skmeans 6 0.719           0.643       0.744         0.0390 0.939   0.758
#&gt; SD:mclust   6 0.807           0.735       0.809         0.0513 0.966   0.871
#&gt; CV:mclust   6 0.695           0.804       0.805         0.0668 0.978   0.910
#&gt; MAD:mclust  6 0.743           0.688       0.799         0.0526 0.922   0.657
#&gt; ATC:mclust  6 0.704           0.640       0.753         0.0394 0.939   0.768
#&gt; SD:kmeans   6 0.665           0.692       0.734         0.0550 0.894   0.627
#&gt; CV:kmeans   6 0.385           0.580       0.662         0.0869 0.916   0.702
#&gt; MAD:kmeans  6 0.635           0.563       0.673         0.0513 0.869   0.556
#&gt; ATC:kmeans  6 0.514           0.317       0.576         0.0453 0.936   0.794
#&gt; SD:pam      6 0.861           0.845       0.919         0.0457 0.958   0.827
#&gt; CV:pam      6 0.600           0.513       0.729         0.0744 0.734   0.434
#&gt; MAD:pam     6 0.879           0.878       0.908         0.0207 0.947   0.791
#&gt; ATC:pam     6 0.733           0.667       0.851         0.0997 0.888   0.654
#&gt; SD:hclust   6 0.832           0.908       0.888         0.0192 0.994   0.977
#&gt; CV:hclust   6 0.925           0.865       0.927         0.0683 0.997   0.994
#&gt; MAD:hclust  6 0.946           0.940       0.967         0.0460 0.944   0.797
#&gt; ATC:hclust  6 0.673           0.648       0.784         0.0791 0.950   0.788
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>



 
## Results for each method


---------------------------------------------------




### SD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.972       0.980         0.4005 0.609   0.609
#> 3 3 0.913           0.949       0.965         0.3402 0.857   0.766
#> 4 4 0.964           0.970       0.977         0.3184 0.816   0.604
#> 5 5 0.852           0.927       0.902         0.0583 0.966   0.881
#> 6 6 0.832           0.908       0.888         0.0192 0.994   0.977
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.0938      0.991 0.012 0.988
#&gt; SRR1617431     2  0.0938      0.991 0.012 0.988
#&gt; SRR1617410     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617411     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617412     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617413     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617414     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617415     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617416     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617417     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617418     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617419     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617420     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617421     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617422     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617423     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617424     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617425     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617427     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617426     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617428     1  0.0376      0.975 0.996 0.004
#&gt; SRR1617429     1  0.0376      0.975 0.996 0.004
#&gt; SRR1617432     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617433     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617434     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617436     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617435     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617437     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617438     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617439     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617440     1  0.3274      0.940 0.940 0.060
#&gt; SRR1617441     1  0.3274      0.940 0.940 0.060
#&gt; SRR1617443     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617442     1  0.0938      0.974 0.988 0.012
#&gt; SRR1617444     1  0.1184      0.977 0.984 0.016
#&gt; SRR1617445     1  0.1184      0.977 0.984 0.016
#&gt; SRR1617446     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617447     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617448     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617449     1  0.0938      0.979 0.988 0.012
#&gt; SRR1617451     2  0.0938      0.991 0.012 0.988
#&gt; SRR1617450     2  0.0938      0.991 0.012 0.988
#&gt; SRR1617452     1  0.7139      0.773 0.804 0.196
#&gt; SRR1617454     2  0.0938      0.991 0.012 0.988
#&gt; SRR1617453     1  0.7139      0.773 0.804 0.196
#&gt; SRR1617456     2  0.0376      0.991 0.004 0.996
#&gt; SRR1617457     2  0.0376      0.991 0.004 0.996
#&gt; SRR1617455     2  0.0938      0.991 0.012 0.988
#&gt; SRR1617458     2  0.0376      0.991 0.004 0.996
#&gt; SRR1617459     2  0.0376      0.991 0.004 0.996
#&gt; SRR1617460     2  0.1184      0.990 0.016 0.984
#&gt; SRR1617461     2  0.1184      0.990 0.016 0.984
#&gt; SRR1617463     2  0.1184      0.990 0.016 0.984
#&gt; SRR1617462     2  0.1184      0.990 0.016 0.984
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.1031      0.980 0.024 0.976 0.000
#&gt; SRR1617431     2  0.1031      0.980 0.024 0.976 0.000
#&gt; SRR1617410     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617412     1  0.2448      0.942 0.924 0.000 0.076
#&gt; SRR1617413     1  0.2448      0.942 0.924 0.000 0.076
#&gt; SRR1617414     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617415     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617416     3  0.3619      0.836 0.136 0.000 0.864
#&gt; SRR1617417     3  0.3619      0.836 0.136 0.000 0.864
#&gt; SRR1617418     1  0.2448      0.942 0.924 0.000 0.076
#&gt; SRR1617419     1  0.2448      0.942 0.924 0.000 0.076
#&gt; SRR1617420     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617421     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617422     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617428     3  0.0829      0.860 0.012 0.004 0.984
#&gt; SRR1617429     3  0.0829      0.860 0.012 0.004 0.984
#&gt; SRR1617432     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617433     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617434     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617436     1  0.2448      0.942 0.924 0.000 0.076
#&gt; SRR1617435     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617437     1  0.2448      0.942 0.924 0.000 0.076
#&gt; SRR1617438     1  0.2448      0.942 0.924 0.000 0.076
#&gt; SRR1617439     1  0.2448      0.942 0.924 0.000 0.076
#&gt; SRR1617440     1  0.4165      0.903 0.876 0.048 0.076
#&gt; SRR1617441     1  0.4165      0.903 0.876 0.048 0.076
#&gt; SRR1617443     1  0.2448      0.942 0.924 0.000 0.076
#&gt; SRR1617442     1  0.2448      0.942 0.924 0.000 0.076
#&gt; SRR1617444     1  0.0237      0.967 0.996 0.004 0.000
#&gt; SRR1617445     1  0.0237      0.967 0.996 0.004 0.000
#&gt; SRR1617446     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.969 1.000 0.000 0.000
#&gt; SRR1617451     2  0.1031      0.980 0.024 0.976 0.000
#&gt; SRR1617450     2  0.1031      0.980 0.024 0.976 0.000
#&gt; SRR1617452     3  0.4346      0.793 0.000 0.184 0.816
#&gt; SRR1617454     2  0.1031      0.980 0.024 0.976 0.000
#&gt; SRR1617453     3  0.4346      0.793 0.000 0.184 0.816
#&gt; SRR1617456     2  0.0237      0.979 0.004 0.996 0.000
#&gt; SRR1617457     2  0.0237      0.979 0.004 0.996 0.000
#&gt; SRR1617455     2  0.1031      0.980 0.024 0.976 0.000
#&gt; SRR1617458     2  0.0237      0.979 0.004 0.996 0.000
#&gt; SRR1617459     2  0.0237      0.979 0.004 0.996 0.000
#&gt; SRR1617460     2  0.0747      0.981 0.016 0.984 0.000
#&gt; SRR1617461     2  0.0747      0.981 0.016 0.984 0.000
#&gt; SRR1617463     2  0.0747      0.981 0.016 0.984 0.000
#&gt; SRR1617462     2  0.0747      0.981 0.016 0.984 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.1406      0.973 0.024 0.960 0.016 0.000
#&gt; SRR1617431     2  0.1406      0.973 0.024 0.960 0.016 0.000
#&gt; SRR1617410     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617412     3  0.0592      0.989 0.016 0.000 0.984 0.000
#&gt; SRR1617413     3  0.0592      0.989 0.016 0.000 0.984 0.000
#&gt; SRR1617414     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617415     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617416     4  0.2868      0.836 0.136 0.000 0.000 0.864
#&gt; SRR1617417     4  0.2868      0.836 0.136 0.000 0.000 0.864
#&gt; SRR1617418     3  0.0592      0.989 0.016 0.000 0.984 0.000
#&gt; SRR1617419     3  0.0592      0.989 0.016 0.000 0.984 0.000
#&gt; SRR1617420     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617421     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617422     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617428     4  0.0657      0.858 0.012 0.000 0.004 0.984
#&gt; SRR1617429     4  0.0657      0.858 0.012 0.000 0.004 0.984
#&gt; SRR1617432     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617433     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617434     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617436     3  0.0592      0.989 0.016 0.000 0.984 0.000
#&gt; SRR1617435     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617437     3  0.0592      0.989 0.016 0.000 0.984 0.000
#&gt; SRR1617438     3  0.0592      0.989 0.016 0.000 0.984 0.000
#&gt; SRR1617439     3  0.0592      0.989 0.016 0.000 0.984 0.000
#&gt; SRR1617440     3  0.1975      0.944 0.016 0.048 0.936 0.000
#&gt; SRR1617441     3  0.1975      0.944 0.016 0.048 0.936 0.000
#&gt; SRR1617443     3  0.0592      0.989 0.016 0.000 0.984 0.000
#&gt; SRR1617442     3  0.0592      0.989 0.016 0.000 0.984 0.000
#&gt; SRR1617444     1  0.0376      0.991 0.992 0.004 0.004 0.000
#&gt; SRR1617445     1  0.0376      0.991 0.992 0.004 0.004 0.000
#&gt; SRR1617446     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.999 1.000 0.000 0.000 0.000
#&gt; SRR1617451     2  0.1406      0.973 0.024 0.960 0.016 0.000
#&gt; SRR1617450     2  0.1406      0.973 0.024 0.960 0.016 0.000
#&gt; SRR1617452     4  0.3444      0.799 0.000 0.184 0.000 0.816
#&gt; SRR1617454     2  0.1406      0.973 0.024 0.960 0.016 0.000
#&gt; SRR1617453     4  0.3444      0.799 0.000 0.184 0.000 0.816
#&gt; SRR1617456     2  0.0000      0.974 0.000 1.000 0.000 0.000
#&gt; SRR1617457     2  0.0000      0.974 0.000 1.000 0.000 0.000
#&gt; SRR1617455     2  0.1406      0.973 0.024 0.960 0.016 0.000
#&gt; SRR1617458     2  0.0000      0.974 0.000 1.000 0.000 0.000
#&gt; SRR1617459     2  0.0000      0.974 0.000 1.000 0.000 0.000
#&gt; SRR1617460     2  0.0469      0.976 0.012 0.988 0.000 0.000
#&gt; SRR1617461     2  0.0469      0.976 0.012 0.988 0.000 0.000
#&gt; SRR1617463     2  0.0469      0.976 0.012 0.988 0.000 0.000
#&gt; SRR1617462     2  0.0469      0.976 0.012 0.988 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     5  0.3983      1.000 0.000 0.340 0.000 0.000 0.660
#&gt; SRR1617431     5  0.3983      1.000 0.000 0.340 0.000 0.000 0.660
#&gt; SRR1617410     1  0.2471      0.919 0.864 0.000 0.000 0.000 0.136
#&gt; SRR1617411     1  0.2471      0.919 0.864 0.000 0.000 0.000 0.136
#&gt; SRR1617412     3  0.0963      0.974 0.000 0.000 0.964 0.000 0.036
#&gt; SRR1617413     3  0.0963      0.974 0.000 0.000 0.964 0.000 0.036
#&gt; SRR1617414     1  0.2471      0.919 0.864 0.000 0.000 0.000 0.136
#&gt; SRR1617415     1  0.2471      0.919 0.864 0.000 0.000 0.000 0.136
#&gt; SRR1617416     4  0.2753      0.794 0.136 0.000 0.000 0.856 0.008
#&gt; SRR1617417     4  0.2753      0.794 0.136 0.000 0.000 0.856 0.008
#&gt; SRR1617418     3  0.0290      0.982 0.000 0.000 0.992 0.000 0.008
#&gt; SRR1617419     3  0.0290      0.982 0.000 0.000 0.992 0.000 0.008
#&gt; SRR1617420     1  0.2471      0.919 0.864 0.000 0.000 0.000 0.136
#&gt; SRR1617421     1  0.2471      0.919 0.864 0.000 0.000 0.000 0.136
#&gt; SRR1617422     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617428     4  0.2891      0.786 0.000 0.000 0.000 0.824 0.176
#&gt; SRR1617429     4  0.2891      0.786 0.000 0.000 0.000 0.824 0.176
#&gt; SRR1617432     1  0.2471      0.919 0.864 0.000 0.000 0.000 0.136
#&gt; SRR1617433     1  0.2471      0.919 0.864 0.000 0.000 0.000 0.136
#&gt; SRR1617434     1  0.2471      0.919 0.864 0.000 0.000 0.000 0.136
#&gt; SRR1617436     3  0.0609      0.980 0.000 0.000 0.980 0.000 0.020
#&gt; SRR1617435     1  0.2471      0.919 0.864 0.000 0.000 0.000 0.136
#&gt; SRR1617437     3  0.0609      0.980 0.000 0.000 0.980 0.000 0.020
#&gt; SRR1617438     3  0.0000      0.983 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617439     3  0.0000      0.983 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617440     3  0.1331      0.955 0.000 0.040 0.952 0.000 0.008
#&gt; SRR1617441     3  0.1331      0.955 0.000 0.040 0.952 0.000 0.008
#&gt; SRR1617443     3  0.0000      0.983 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617442     3  0.0000      0.983 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617444     1  0.0324      0.927 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1617445     1  0.0324      0.927 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1617446     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.932 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617451     5  0.3983      1.000 0.000 0.340 0.000 0.000 0.660
#&gt; SRR1617450     5  0.3983      1.000 0.000 0.340 0.000 0.000 0.660
#&gt; SRR1617452     4  0.2966      0.745 0.000 0.184 0.000 0.816 0.000
#&gt; SRR1617454     5  0.3983      1.000 0.000 0.340 0.000 0.000 0.660
#&gt; SRR1617453     4  0.2966      0.745 0.000 0.184 0.000 0.816 0.000
#&gt; SRR1617456     2  0.0000      0.920 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617457     2  0.0000      0.920 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617455     5  0.3983      1.000 0.000 0.340 0.000 0.000 0.660
#&gt; SRR1617458     2  0.0000      0.920 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617459     2  0.0000      0.920 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617460     2  0.2069      0.915 0.012 0.912 0.000 0.000 0.076
#&gt; SRR1617461     2  0.2069      0.915 0.012 0.912 0.000 0.000 0.076
#&gt; SRR1617463     2  0.2069      0.915 0.012 0.912 0.000 0.000 0.076
#&gt; SRR1617462     2  0.2069      0.915 0.012 0.912 0.000 0.000 0.076
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.1814      1.000 0.000 0.900 0.000 0.000 0.000 0.100
#&gt; SRR1617431     2  0.1814      1.000 0.000 0.900 0.000 0.000 0.000 0.100
#&gt; SRR1617410     1  0.2536      0.917 0.864 0.020 0.000 0.000 0.116 0.000
#&gt; SRR1617411     1  0.2536      0.917 0.864 0.020 0.000 0.000 0.116 0.000
#&gt; SRR1617412     3  0.2474      0.919 0.000 0.080 0.884 0.032 0.004 0.000
#&gt; SRR1617413     3  0.2474      0.919 0.000 0.080 0.884 0.032 0.004 0.000
#&gt; SRR1617414     1  0.2536      0.917 0.864 0.020 0.000 0.000 0.116 0.000
#&gt; SRR1617415     1  0.2536      0.917 0.864 0.020 0.000 0.000 0.116 0.000
#&gt; SRR1617416     5  0.2473      0.795 0.136 0.000 0.000 0.008 0.856 0.000
#&gt; SRR1617417     5  0.2473      0.795 0.136 0.000 0.000 0.008 0.856 0.000
#&gt; SRR1617418     3  0.0260      0.958 0.000 0.008 0.992 0.000 0.000 0.000
#&gt; SRR1617419     3  0.0260      0.958 0.000 0.008 0.992 0.000 0.000 0.000
#&gt; SRR1617420     1  0.2536      0.917 0.864 0.020 0.000 0.000 0.116 0.000
#&gt; SRR1617421     1  0.2536      0.917 0.864 0.020 0.000 0.000 0.116 0.000
#&gt; SRR1617422     1  0.0000      0.929 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.929 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.929 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.929 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.929 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.929 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617428     4  0.0790      1.000 0.000 0.000 0.000 0.968 0.032 0.000
#&gt; SRR1617429     4  0.0790      1.000 0.000 0.000 0.000 0.968 0.032 0.000
#&gt; SRR1617432     1  0.2536      0.917 0.864 0.020 0.000 0.000 0.116 0.000
#&gt; SRR1617433     1  0.2536      0.917 0.864 0.020 0.000 0.000 0.116 0.000
#&gt; SRR1617434     1  0.2536      0.917 0.864 0.020 0.000 0.000 0.116 0.000
#&gt; SRR1617436     3  0.1826      0.940 0.000 0.052 0.924 0.020 0.004 0.000
#&gt; SRR1617435     1  0.2536      0.917 0.864 0.020 0.000 0.000 0.116 0.000
#&gt; SRR1617437     3  0.1826      0.940 0.000 0.052 0.924 0.020 0.004 0.000
#&gt; SRR1617438     3  0.0000      0.958 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617439     3  0.0000      0.958 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617440     3  0.1196      0.934 0.000 0.008 0.952 0.000 0.000 0.040
#&gt; SRR1617441     3  0.1196      0.934 0.000 0.008 0.952 0.000 0.000 0.040
#&gt; SRR1617443     3  0.0260      0.959 0.000 0.008 0.992 0.000 0.000 0.000
#&gt; SRR1617442     3  0.0260      0.959 0.000 0.008 0.992 0.000 0.000 0.000
#&gt; SRR1617444     1  0.0291      0.925 0.992 0.004 0.004 0.000 0.000 0.000
#&gt; SRR1617445     1  0.0291      0.925 0.992 0.004 0.004 0.000 0.000 0.000
#&gt; SRR1617446     1  0.0000      0.929 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.929 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.929 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.929 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617451     2  0.1814      1.000 0.000 0.900 0.000 0.000 0.000 0.100
#&gt; SRR1617450     2  0.1814      1.000 0.000 0.900 0.000 0.000 0.000 0.100
#&gt; SRR1617452     5  0.3364      0.792 0.000 0.036 0.000 0.012 0.820 0.132
#&gt; SRR1617454     2  0.1814      1.000 0.000 0.900 0.000 0.000 0.000 0.100
#&gt; SRR1617453     5  0.3364      0.792 0.000 0.036 0.000 0.012 0.820 0.132
#&gt; SRR1617456     6  0.0000      0.798 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617457     6  0.0000      0.798 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617455     2  0.1814      1.000 0.000 0.900 0.000 0.000 0.000 0.100
#&gt; SRR1617458     6  0.0000      0.798 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617459     6  0.0000      0.798 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617460     6  0.3541      0.757 0.012 0.260 0.000 0.000 0.000 0.728
#&gt; SRR1617461     6  0.3541      0.757 0.012 0.260 0.000 0.000 0.000 0.728
#&gt; SRR1617463     6  0.3541      0.757 0.012 0.260 0.000 0.000 0.000 0.728
#&gt; SRR1617462     6  0.3541      0.757 0.012 0.260 0.000 0.000 0.000 0.728
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.249           0.703       0.814         0.4376 0.525   0.525
#> 3 3 0.382           0.817       0.814         0.3824 0.760   0.565
#> 4 4 0.502           0.723       0.789         0.1504 0.983   0.951
#> 5 5 0.647           0.490       0.722         0.0849 0.913   0.739
#> 6 6 0.665           0.692       0.734         0.0550 0.894   0.627
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.7950      0.821 0.240 0.760
#&gt; SRR1617431     2  0.7950      0.821 0.240 0.760
#&gt; SRR1617410     1  0.0000      0.806 1.000 0.000
#&gt; SRR1617411     1  0.0000      0.806 1.000 0.000
#&gt; SRR1617412     1  0.9608      0.545 0.616 0.384
#&gt; SRR1617413     1  0.9608      0.545 0.616 0.384
#&gt; SRR1617414     1  0.1184      0.797 0.984 0.016
#&gt; SRR1617415     1  0.1184      0.797 0.984 0.016
#&gt; SRR1617416     1  0.4022      0.762 0.920 0.080
#&gt; SRR1617417     1  0.4022      0.762 0.920 0.080
#&gt; SRR1617418     1  0.9775      0.510 0.588 0.412
#&gt; SRR1617419     1  0.9775      0.510 0.588 0.412
#&gt; SRR1617420     1  0.1184      0.804 0.984 0.016
#&gt; SRR1617421     1  0.1184      0.804 0.984 0.016
#&gt; SRR1617422     1  0.1184      0.797 0.984 0.016
#&gt; SRR1617423     1  0.1184      0.797 0.984 0.016
#&gt; SRR1617424     1  0.0376      0.806 0.996 0.004
#&gt; SRR1617425     1  0.0376      0.806 0.996 0.004
#&gt; SRR1617427     1  0.0672      0.806 0.992 0.008
#&gt; SRR1617426     1  0.0672      0.806 0.992 0.008
#&gt; SRR1617428     2  0.9977      0.144 0.472 0.528
#&gt; SRR1617429     2  0.9977      0.144 0.472 0.528
#&gt; SRR1617432     1  0.0000      0.806 1.000 0.000
#&gt; SRR1617433     1  0.0000      0.806 1.000 0.000
#&gt; SRR1617434     1  0.0938      0.804 0.988 0.012
#&gt; SRR1617436     1  0.9775      0.510 0.588 0.412
#&gt; SRR1617435     1  0.0938      0.804 0.988 0.012
#&gt; SRR1617437     1  0.9775      0.510 0.588 0.412
#&gt; SRR1617438     1  0.9775      0.510 0.588 0.412
#&gt; SRR1617439     1  0.9775      0.510 0.588 0.412
#&gt; SRR1617440     2  0.8386      0.470 0.268 0.732
#&gt; SRR1617441     2  0.8386      0.470 0.268 0.732
#&gt; SRR1617443     1  0.9608      0.545 0.616 0.384
#&gt; SRR1617442     1  0.9608      0.545 0.616 0.384
#&gt; SRR1617444     1  0.5629      0.690 0.868 0.132
#&gt; SRR1617445     1  0.5629      0.690 0.868 0.132
#&gt; SRR1617446     1  0.0376      0.806 0.996 0.004
#&gt; SRR1617447     1  0.0376      0.806 0.996 0.004
#&gt; SRR1617448     1  0.0376      0.806 0.996 0.004
#&gt; SRR1617449     1  0.0376      0.806 0.996 0.004
#&gt; SRR1617451     2  0.7950      0.821 0.240 0.760
#&gt; SRR1617450     2  0.7950      0.821 0.240 0.760
#&gt; SRR1617452     2  0.8144      0.712 0.252 0.748
#&gt; SRR1617454     2  0.7950      0.821 0.240 0.760
#&gt; SRR1617453     2  0.8144      0.712 0.252 0.748
#&gt; SRR1617456     2  0.7674      0.818 0.224 0.776
#&gt; SRR1617457     2  0.7674      0.818 0.224 0.776
#&gt; SRR1617455     2  0.7950      0.821 0.240 0.760
#&gt; SRR1617458     2  0.7674      0.818 0.224 0.776
#&gt; SRR1617459     2  0.7674      0.818 0.224 0.776
#&gt; SRR1617460     2  0.9427      0.724 0.360 0.640
#&gt; SRR1617461     2  0.9427      0.724 0.360 0.640
#&gt; SRR1617463     2  0.9460      0.720 0.364 0.636
#&gt; SRR1617462     2  0.9460      0.720 0.364 0.636
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2   0.441      0.888 0.104 0.860 0.036
#&gt; SRR1617431     2   0.441      0.888 0.104 0.860 0.036
#&gt; SRR1617410     1   0.164      0.885 0.956 0.000 0.044
#&gt; SRR1617411     1   0.164      0.885 0.956 0.000 0.044
#&gt; SRR1617412     3   0.753      0.827 0.252 0.084 0.664
#&gt; SRR1617413     3   0.753      0.827 0.252 0.084 0.664
#&gt; SRR1617414     1   0.165      0.887 0.960 0.004 0.036
#&gt; SRR1617415     1   0.165      0.887 0.960 0.004 0.036
#&gt; SRR1617416     1   0.666      0.650 0.704 0.044 0.252
#&gt; SRR1617417     1   0.666      0.650 0.704 0.044 0.252
#&gt; SRR1617418     3   0.753      0.840 0.228 0.096 0.676
#&gt; SRR1617419     3   0.753      0.840 0.228 0.096 0.676
#&gt; SRR1617420     1   0.186      0.886 0.948 0.000 0.052
#&gt; SRR1617421     1   0.186      0.886 0.948 0.000 0.052
#&gt; SRR1617422     1   0.331      0.887 0.908 0.028 0.064
#&gt; SRR1617423     1   0.331      0.887 0.908 0.028 0.064
#&gt; SRR1617424     1   0.270      0.890 0.928 0.016 0.056
#&gt; SRR1617425     1   0.270      0.890 0.928 0.016 0.056
#&gt; SRR1617427     1   0.265      0.890 0.928 0.012 0.060
#&gt; SRR1617426     1   0.265      0.890 0.928 0.012 0.060
#&gt; SRR1617428     3   0.944      0.249 0.240 0.256 0.504
#&gt; SRR1617429     3   0.944      0.249 0.240 0.256 0.504
#&gt; SRR1617432     1   0.164      0.886 0.956 0.000 0.044
#&gt; SRR1617433     1   0.164      0.886 0.956 0.000 0.044
#&gt; SRR1617434     1   0.199      0.884 0.948 0.004 0.048
#&gt; SRR1617436     3   0.718      0.838 0.240 0.072 0.688
#&gt; SRR1617435     1   0.199      0.884 0.948 0.004 0.048
#&gt; SRR1617437     3   0.718      0.838 0.240 0.072 0.688
#&gt; SRR1617438     3   0.711      0.840 0.224 0.076 0.700
#&gt; SRR1617439     3   0.711      0.840 0.224 0.076 0.700
#&gt; SRR1617440     3   0.741      0.679 0.092 0.224 0.684
#&gt; SRR1617441     3   0.741      0.679 0.092 0.224 0.684
#&gt; SRR1617443     3   0.704      0.832 0.252 0.060 0.688
#&gt; SRR1617442     3   0.704      0.832 0.252 0.060 0.688
#&gt; SRR1617444     1   0.602      0.761 0.784 0.076 0.140
#&gt; SRR1617445     1   0.602      0.761 0.784 0.076 0.140
#&gt; SRR1617446     1   0.318      0.887 0.908 0.016 0.076
#&gt; SRR1617447     1   0.318      0.887 0.908 0.016 0.076
#&gt; SRR1617448     1   0.309      0.888 0.912 0.016 0.072
#&gt; SRR1617449     1   0.309      0.888 0.912 0.016 0.072
#&gt; SRR1617451     2   0.354      0.893 0.100 0.888 0.012
#&gt; SRR1617450     2   0.354      0.893 0.100 0.888 0.012
#&gt; SRR1617452     2   0.816      0.501 0.084 0.568 0.348
#&gt; SRR1617454     2   0.338      0.894 0.100 0.892 0.008
#&gt; SRR1617453     2   0.816      0.501 0.084 0.568 0.348
#&gt; SRR1617456     2   0.473      0.884 0.088 0.852 0.060
#&gt; SRR1617457     2   0.473      0.884 0.088 0.852 0.060
#&gt; SRR1617455     2   0.338      0.894 0.100 0.892 0.008
#&gt; SRR1617458     2   0.482      0.883 0.088 0.848 0.064
#&gt; SRR1617459     2   0.482      0.883 0.088 0.848 0.064
#&gt; SRR1617460     2   0.641      0.841 0.144 0.764 0.092
#&gt; SRR1617461     2   0.641      0.841 0.144 0.764 0.092
#&gt; SRR1617463     2   0.489      0.878 0.124 0.836 0.040
#&gt; SRR1617462     2   0.489      0.878 0.124 0.836 0.040
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2   0.463     0.7662 0.040 0.828 0.056 0.076
#&gt; SRR1617431     2   0.463     0.7662 0.040 0.828 0.056 0.076
#&gt; SRR1617410     1   0.507     0.7081 0.740 0.000 0.052 0.208
#&gt; SRR1617411     1   0.507     0.7081 0.740 0.000 0.052 0.208
#&gt; SRR1617412     3   0.483     0.8480 0.100 0.020 0.808 0.072
#&gt; SRR1617413     3   0.483     0.8480 0.100 0.020 0.808 0.072
#&gt; SRR1617414     1   0.545     0.6879 0.700 0.000 0.056 0.244
#&gt; SRR1617415     1   0.545     0.6879 0.700 0.000 0.056 0.244
#&gt; SRR1617416     1   0.702     0.1940 0.556 0.016 0.088 0.340
#&gt; SRR1617417     1   0.702     0.1940 0.556 0.016 0.088 0.340
#&gt; SRR1617418     3   0.355     0.8879 0.096 0.016 0.868 0.020
#&gt; SRR1617419     3   0.355     0.8879 0.096 0.016 0.868 0.020
#&gt; SRR1617420     1   0.522     0.7063 0.728 0.000 0.056 0.216
#&gt; SRR1617421     1   0.522     0.7063 0.728 0.000 0.056 0.216
#&gt; SRR1617422     1   0.244     0.7319 0.916 0.012 0.068 0.004
#&gt; SRR1617423     1   0.244     0.7319 0.916 0.012 0.068 0.004
#&gt; SRR1617424     1   0.182     0.7358 0.936 0.004 0.060 0.000
#&gt; SRR1617425     1   0.182     0.7358 0.936 0.004 0.060 0.000
#&gt; SRR1617427     1   0.247     0.7363 0.916 0.000 0.056 0.028
#&gt; SRR1617426     1   0.247     0.7363 0.916 0.000 0.056 0.028
#&gt; SRR1617428     4   0.910     1.0000 0.168 0.116 0.268 0.448
#&gt; SRR1617429     4   0.910     1.0000 0.168 0.116 0.268 0.448
#&gt; SRR1617432     1   0.526     0.6901 0.700 0.000 0.040 0.260
#&gt; SRR1617433     1   0.526     0.6901 0.700 0.000 0.040 0.260
#&gt; SRR1617434     1   0.529     0.7006 0.720 0.000 0.056 0.224
#&gt; SRR1617436     3   0.297     0.8927 0.096 0.000 0.884 0.020
#&gt; SRR1617435     1   0.529     0.7006 0.720 0.000 0.056 0.224
#&gt; SRR1617437     3   0.297     0.8927 0.096 0.000 0.884 0.020
#&gt; SRR1617438     3   0.297     0.8925 0.096 0.000 0.884 0.020
#&gt; SRR1617439     3   0.297     0.8925 0.096 0.000 0.884 0.020
#&gt; SRR1617440     3   0.582     0.6661 0.056 0.108 0.760 0.076
#&gt; SRR1617441     3   0.582     0.6661 0.056 0.108 0.760 0.076
#&gt; SRR1617443     3   0.298     0.8922 0.092 0.004 0.888 0.016
#&gt; SRR1617442     3   0.298     0.8922 0.092 0.004 0.888 0.016
#&gt; SRR1617444     1   0.508     0.6230 0.804 0.048 0.088 0.060
#&gt; SRR1617445     1   0.508     0.6230 0.804 0.048 0.088 0.060
#&gt; SRR1617446     1   0.280     0.7250 0.900 0.008 0.080 0.012
#&gt; SRR1617447     1   0.280     0.7250 0.900 0.008 0.080 0.012
#&gt; SRR1617448     1   0.280     0.7250 0.900 0.008 0.080 0.012
#&gt; SRR1617449     1   0.280     0.7250 0.900 0.008 0.080 0.012
#&gt; SRR1617451     2   0.376     0.7813 0.032 0.872 0.048 0.048
#&gt; SRR1617450     2   0.376     0.7813 0.032 0.872 0.048 0.048
#&gt; SRR1617452     2   0.776     0.0868 0.032 0.432 0.108 0.428
#&gt; SRR1617454     2   0.231     0.7939 0.032 0.932 0.016 0.020
#&gt; SRR1617453     2   0.776     0.0868 0.032 0.432 0.108 0.428
#&gt; SRR1617456     2   0.505     0.7735 0.028 0.788 0.044 0.140
#&gt; SRR1617457     2   0.505     0.7735 0.028 0.788 0.044 0.140
#&gt; SRR1617455     2   0.231     0.7939 0.032 0.932 0.016 0.020
#&gt; SRR1617458     2   0.521     0.7712 0.028 0.772 0.040 0.160
#&gt; SRR1617459     2   0.521     0.7712 0.028 0.772 0.040 0.160
#&gt; SRR1617460     2   0.533     0.7439 0.076 0.756 0.008 0.160
#&gt; SRR1617461     2   0.533     0.7439 0.076 0.756 0.008 0.160
#&gt; SRR1617463     2   0.347     0.7727 0.072 0.868 0.000 0.060
#&gt; SRR1617462     2   0.347     0.7727 0.072 0.868 0.000 0.060
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     2  0.4148      0.731 0.008 0.816 0.016 0.056 0.104
#&gt; SRR1617431     2  0.4148      0.731 0.008 0.816 0.016 0.056 0.104
#&gt; SRR1617410     1  0.1721      0.489 0.944 0.000 0.020 0.020 0.016
#&gt; SRR1617411     1  0.1721      0.489 0.944 0.000 0.020 0.020 0.016
#&gt; SRR1617412     3  0.4368      0.836 0.032 0.000 0.796 0.056 0.116
#&gt; SRR1617413     3  0.4368      0.836 0.032 0.000 0.796 0.056 0.116
#&gt; SRR1617414     1  0.2647      0.463 0.892 0.000 0.008 0.024 0.076
#&gt; SRR1617415     1  0.2647      0.463 0.892 0.000 0.008 0.024 0.076
#&gt; SRR1617416     4  0.7749      0.148 0.308 0.004 0.056 0.408 0.224
#&gt; SRR1617417     4  0.7749      0.148 0.308 0.004 0.056 0.408 0.224
#&gt; SRR1617418     3  0.3161      0.874 0.016 0.008 0.880 0.048 0.048
#&gt; SRR1617419     3  0.3161      0.874 0.016 0.008 0.880 0.048 0.048
#&gt; SRR1617420     1  0.1597      0.490 0.948 0.000 0.024 0.008 0.020
#&gt; SRR1617421     1  0.1597      0.490 0.948 0.000 0.024 0.008 0.020
#&gt; SRR1617422     1  0.5869     -0.309 0.512 0.008 0.048 0.012 0.420
#&gt; SRR1617423     1  0.5869     -0.309 0.512 0.008 0.048 0.012 0.420
#&gt; SRR1617424     1  0.5590     -0.199 0.540 0.000 0.056 0.008 0.396
#&gt; SRR1617425     1  0.5590     -0.199 0.540 0.000 0.056 0.008 0.396
#&gt; SRR1617427     1  0.5612     -0.209 0.520 0.000 0.048 0.012 0.420
#&gt; SRR1617426     1  0.5612     -0.209 0.520 0.000 0.048 0.012 0.420
#&gt; SRR1617428     4  0.8659      0.543 0.104 0.060 0.140 0.432 0.264
#&gt; SRR1617429     4  0.8659      0.543 0.104 0.060 0.140 0.432 0.264
#&gt; SRR1617432     1  0.2036      0.474 0.928 0.000 0.008 0.028 0.036
#&gt; SRR1617433     1  0.2036      0.474 0.928 0.000 0.008 0.028 0.036
#&gt; SRR1617434     1  0.0898      0.492 0.972 0.000 0.020 0.008 0.000
#&gt; SRR1617436     3  0.3104      0.859 0.016 0.004 0.880 0.044 0.056
#&gt; SRR1617435     1  0.0898      0.492 0.972 0.000 0.020 0.008 0.000
#&gt; SRR1617437     3  0.3104      0.859 0.016 0.004 0.880 0.044 0.056
#&gt; SRR1617438     3  0.1419      0.883 0.016 0.000 0.956 0.012 0.016
#&gt; SRR1617439     3  0.1419      0.883 0.016 0.000 0.956 0.012 0.016
#&gt; SRR1617440     3  0.4795      0.780 0.008 0.056 0.788 0.084 0.064
#&gt; SRR1617441     3  0.4795      0.780 0.008 0.056 0.788 0.084 0.064
#&gt; SRR1617443     3  0.2002      0.883 0.028 0.000 0.932 0.020 0.020
#&gt; SRR1617442     3  0.2002      0.883 0.028 0.000 0.932 0.020 0.020
#&gt; SRR1617444     5  0.7419      1.000 0.360 0.020 0.100 0.056 0.464
#&gt; SRR1617445     5  0.7419      1.000 0.360 0.020 0.100 0.056 0.464
#&gt; SRR1617446     1  0.6062     -0.312 0.508 0.000 0.064 0.024 0.404
#&gt; SRR1617447     1  0.6062     -0.312 0.508 0.000 0.064 0.024 0.404
#&gt; SRR1617448     1  0.6091     -0.423 0.480 0.000 0.064 0.024 0.432
#&gt; SRR1617449     1  0.6091     -0.423 0.480 0.000 0.064 0.024 0.432
#&gt; SRR1617451     2  0.2788      0.776 0.004 0.892 0.008 0.032 0.064
#&gt; SRR1617450     2  0.2788      0.776 0.004 0.892 0.008 0.032 0.064
#&gt; SRR1617452     4  0.6995      0.327 0.028 0.252 0.096 0.580 0.044
#&gt; SRR1617454     2  0.1130      0.789 0.004 0.968 0.004 0.012 0.012
#&gt; SRR1617453     4  0.6995      0.327 0.028 0.252 0.096 0.580 0.044
#&gt; SRR1617456     2  0.5128      0.723 0.004 0.716 0.016 0.200 0.064
#&gt; SRR1617457     2  0.5128      0.723 0.004 0.716 0.016 0.200 0.064
#&gt; SRR1617455     2  0.1130      0.789 0.004 0.968 0.004 0.012 0.012
#&gt; SRR1617458     2  0.5189      0.721 0.004 0.704 0.016 0.216 0.060
#&gt; SRR1617459     2  0.5189      0.721 0.004 0.704 0.016 0.216 0.060
#&gt; SRR1617460     2  0.5833      0.640 0.036 0.668 0.000 0.196 0.100
#&gt; SRR1617461     2  0.5833      0.640 0.036 0.668 0.000 0.196 0.100
#&gt; SRR1617463     2  0.3574      0.751 0.004 0.836 0.000 0.088 0.072
#&gt; SRR1617462     2  0.3574      0.751 0.004 0.836 0.000 0.088 0.072
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2   0.410      0.609 0.024 0.812 0.012 0.096 0.020 0.036
#&gt; SRR1617431     2   0.410      0.609 0.024 0.812 0.012 0.096 0.020 0.036
#&gt; SRR1617410     5   0.534      0.816 0.312 0.000 0.012 0.024 0.604 0.048
#&gt; SRR1617411     5   0.534      0.816 0.312 0.000 0.012 0.024 0.604 0.048
#&gt; SRR1617412     3   0.476      0.753 0.020 0.000 0.732 0.020 0.060 0.168
#&gt; SRR1617413     3   0.476      0.753 0.020 0.000 0.732 0.020 0.060 0.168
#&gt; SRR1617414     5   0.554      0.777 0.244 0.000 0.004 0.036 0.628 0.088
#&gt; SRR1617415     5   0.554      0.777 0.244 0.000 0.004 0.036 0.628 0.088
#&gt; SRR1617416     1   0.749      0.214 0.444 0.000 0.036 0.244 0.200 0.076
#&gt; SRR1617417     1   0.749      0.214 0.444 0.000 0.036 0.244 0.200 0.076
#&gt; SRR1617418     3   0.320      0.826 0.036 0.000 0.860 0.008 0.032 0.064
#&gt; SRR1617419     3   0.320      0.826 0.036 0.000 0.860 0.008 0.032 0.064
#&gt; SRR1617420     5   0.536      0.818 0.304 0.000 0.020 0.036 0.612 0.028
#&gt; SRR1617421     5   0.536      0.818 0.304 0.000 0.020 0.036 0.612 0.028
#&gt; SRR1617422     1   0.282      0.748 0.872 0.008 0.000 0.040 0.076 0.004
#&gt; SRR1617423     1   0.282      0.748 0.872 0.008 0.000 0.040 0.076 0.004
#&gt; SRR1617424     1   0.259      0.746 0.892 0.004 0.012 0.016 0.068 0.008
#&gt; SRR1617425     1   0.259      0.746 0.892 0.004 0.012 0.016 0.068 0.008
#&gt; SRR1617427     1   0.422      0.695 0.792 0.000 0.020 0.024 0.104 0.060
#&gt; SRR1617426     1   0.422      0.695 0.792 0.000 0.020 0.024 0.104 0.060
#&gt; SRR1617428     4   0.570      0.645 0.092 0.076 0.092 0.704 0.028 0.008
#&gt; SRR1617429     4   0.559      0.645 0.092 0.076 0.092 0.708 0.028 0.004
#&gt; SRR1617432     5   0.534      0.797 0.228 0.000 0.004 0.032 0.652 0.084
#&gt; SRR1617433     5   0.534      0.797 0.228 0.000 0.004 0.032 0.652 0.084
#&gt; SRR1617434     5   0.450      0.831 0.284 0.000 0.012 0.024 0.672 0.008
#&gt; SRR1617436     3   0.466      0.781 0.044 0.000 0.772 0.084 0.028 0.072
#&gt; SRR1617435     5   0.450      0.831 0.284 0.000 0.012 0.024 0.672 0.008
#&gt; SRR1617437     3   0.466      0.781 0.044 0.000 0.772 0.084 0.028 0.072
#&gt; SRR1617438     3   0.204      0.830 0.048 0.000 0.920 0.016 0.008 0.008
#&gt; SRR1617439     3   0.204      0.830 0.048 0.000 0.920 0.016 0.008 0.008
#&gt; SRR1617440     3   0.588      0.667 0.044 0.096 0.688 0.048 0.008 0.116
#&gt; SRR1617441     3   0.588      0.667 0.044 0.096 0.688 0.048 0.008 0.116
#&gt; SRR1617443     3   0.248      0.826 0.028 0.000 0.904 0.012 0.024 0.032
#&gt; SRR1617442     3   0.248      0.826 0.028 0.000 0.904 0.012 0.024 0.032
#&gt; SRR1617444     1   0.438      0.680 0.800 0.016 0.028 0.092 0.028 0.036
#&gt; SRR1617445     1   0.438      0.680 0.800 0.016 0.028 0.092 0.028 0.036
#&gt; SRR1617446     1   0.214      0.762 0.924 0.004 0.016 0.020 0.016 0.020
#&gt; SRR1617447     1   0.214      0.762 0.924 0.004 0.016 0.020 0.016 0.020
#&gt; SRR1617448     1   0.160      0.763 0.944 0.004 0.012 0.020 0.000 0.020
#&gt; SRR1617449     1   0.160      0.763 0.944 0.004 0.012 0.020 0.000 0.020
#&gt; SRR1617451     2   0.228      0.666 0.012 0.912 0.000 0.024 0.012 0.040
#&gt; SRR1617450     2   0.228      0.666 0.012 0.912 0.000 0.024 0.012 0.040
#&gt; SRR1617452     4   0.840      0.564 0.016 0.176 0.100 0.416 0.088 0.204
#&gt; SRR1617454     2   0.081      0.678 0.008 0.976 0.004 0.008 0.000 0.004
#&gt; SRR1617453     4   0.840      0.564 0.016 0.176 0.100 0.416 0.088 0.204
#&gt; SRR1617456     2   0.439      0.566 0.000 0.604 0.008 0.012 0.004 0.372
#&gt; SRR1617457     2   0.439      0.566 0.000 0.604 0.008 0.012 0.004 0.372
#&gt; SRR1617455     2   0.081      0.678 0.008 0.976 0.004 0.008 0.000 0.004
#&gt; SRR1617458     2   0.433      0.558 0.000 0.576 0.008 0.012 0.000 0.404
#&gt; SRR1617459     2   0.433      0.558 0.000 0.576 0.008 0.012 0.000 0.404
#&gt; SRR1617460     2   0.697      0.469 0.076 0.556 0.000 0.156 0.044 0.168
#&gt; SRR1617461     2   0.697      0.469 0.076 0.556 0.000 0.156 0.044 0.168
#&gt; SRR1617463     2   0.541      0.589 0.032 0.708 0.004 0.132 0.028 0.096
#&gt; SRR1617462     2   0.541      0.589 0.032 0.708 0.004 0.132 0.028 0.096
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.683           0.948       0.968         0.4942 0.508   0.508
#> 3 3 0.958           0.917       0.963         0.3488 0.768   0.567
#> 4 4 0.855           0.930       0.942         0.1337 0.877   0.647
#> 5 5 0.879           0.903       0.929         0.0639 0.939   0.755
#> 6 6 0.867           0.840       0.868         0.0352 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617431     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617410     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617411     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617412     1  0.5519      0.894 0.872 0.128
#&gt; SRR1617413     1  0.5519      0.894 0.872 0.128
#&gt; SRR1617414     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617415     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617416     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617417     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617418     1  0.5737      0.888 0.864 0.136
#&gt; SRR1617419     1  0.5737      0.888 0.864 0.136
#&gt; SRR1617420     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617421     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617422     1  0.2236      0.932 0.964 0.036
#&gt; SRR1617423     1  0.2236      0.932 0.964 0.036
#&gt; SRR1617424     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617425     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617427     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617426     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617428     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617429     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617432     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617433     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617434     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617436     1  0.5629      0.891 0.868 0.132
#&gt; SRR1617435     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617437     1  0.5629      0.891 0.868 0.132
#&gt; SRR1617438     1  0.5737      0.888 0.864 0.136
#&gt; SRR1617439     1  0.5737      0.888 0.864 0.136
#&gt; SRR1617440     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617441     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617443     1  0.5519      0.894 0.872 0.128
#&gt; SRR1617442     1  0.5519      0.894 0.872 0.128
#&gt; SRR1617444     2  0.6048      0.837 0.148 0.852
#&gt; SRR1617445     2  0.6048      0.837 0.148 0.852
#&gt; SRR1617446     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617447     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617448     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617449     1  0.0000      0.952 1.000 0.000
#&gt; SRR1617451     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617450     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617452     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617454     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617453     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617456     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617457     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617455     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617458     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617459     2  0.0000      0.984 0.000 1.000
#&gt; SRR1617460     2  0.0376      0.982 0.004 0.996
#&gt; SRR1617461     2  0.0376      0.982 0.004 0.996
#&gt; SRR1617463     2  0.0376      0.982 0.004 0.996
#&gt; SRR1617462     2  0.0376      0.982 0.004 0.996
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617431     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617410     1  0.0424      0.991 0.992 0.000 0.008
#&gt; SRR1617411     1  0.0424      0.991 0.992 0.000 0.008
#&gt; SRR1617412     3  0.0000      0.955 0.000 0.000 1.000
#&gt; SRR1617413     3  0.0000      0.955 0.000 0.000 1.000
#&gt; SRR1617414     1  0.0237      0.992 0.996 0.000 0.004
#&gt; SRR1617415     1  0.0237      0.992 0.996 0.000 0.004
#&gt; SRR1617416     1  0.1860      0.950 0.948 0.000 0.052
#&gt; SRR1617417     1  0.1860      0.950 0.948 0.000 0.052
#&gt; SRR1617418     3  0.0000      0.955 0.000 0.000 1.000
#&gt; SRR1617419     3  0.0000      0.955 0.000 0.000 1.000
#&gt; SRR1617420     1  0.0424      0.991 0.992 0.000 0.008
#&gt; SRR1617421     1  0.0424      0.991 0.992 0.000 0.008
#&gt; SRR1617422     1  0.0000      0.992 1.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.992 1.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.992 1.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.992 1.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.992 1.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.992 1.000 0.000 0.000
#&gt; SRR1617428     3  0.5138      0.670 0.000 0.252 0.748
#&gt; SRR1617429     3  0.5138      0.670 0.000 0.252 0.748
#&gt; SRR1617432     1  0.0424      0.991 0.992 0.000 0.008
#&gt; SRR1617433     1  0.0424      0.991 0.992 0.000 0.008
#&gt; SRR1617434     1  0.0424      0.991 0.992 0.000 0.008
#&gt; SRR1617436     3  0.0000      0.955 0.000 0.000 1.000
#&gt; SRR1617435     1  0.0424      0.991 0.992 0.000 0.008
#&gt; SRR1617437     3  0.0000      0.955 0.000 0.000 1.000
#&gt; SRR1617438     3  0.0000      0.955 0.000 0.000 1.000
#&gt; SRR1617439     3  0.0000      0.955 0.000 0.000 1.000
#&gt; SRR1617440     3  0.0592      0.948 0.000 0.012 0.988
#&gt; SRR1617441     3  0.0592      0.948 0.000 0.012 0.988
#&gt; SRR1617443     3  0.0000      0.955 0.000 0.000 1.000
#&gt; SRR1617442     3  0.0000      0.955 0.000 0.000 1.000
#&gt; SRR1617444     2  0.9907      0.132 0.288 0.400 0.312
#&gt; SRR1617445     2  0.9907      0.132 0.288 0.400 0.312
#&gt; SRR1617446     1  0.0000      0.992 1.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.992 1.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.992 1.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.992 1.000 0.000 0.000
#&gt; SRR1617451     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617450     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617452     2  0.1411      0.895 0.000 0.964 0.036
#&gt; SRR1617454     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617453     2  0.1411      0.895 0.000 0.964 0.036
#&gt; SRR1617456     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617457     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617455     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617458     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617459     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617460     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617461     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617463     2  0.0000      0.925 0.000 1.000 0.000
#&gt; SRR1617462     2  0.0000      0.925 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.0188      0.967 0.004 0.996 0.000 0.000
#&gt; SRR1617431     2  0.0188      0.967 0.004 0.996 0.000 0.000
#&gt; SRR1617410     4  0.0336      0.995 0.008 0.000 0.000 0.992
#&gt; SRR1617411     4  0.0336      0.995 0.008 0.000 0.000 0.992
#&gt; SRR1617412     3  0.0000      0.955 0.000 0.000 1.000 0.000
#&gt; SRR1617413     3  0.0000      0.955 0.000 0.000 1.000 0.000
#&gt; SRR1617414     4  0.0707      0.986 0.020 0.000 0.000 0.980
#&gt; SRR1617415     4  0.0707      0.986 0.020 0.000 0.000 0.980
#&gt; SRR1617416     1  0.5090      0.679 0.728 0.000 0.044 0.228
#&gt; SRR1617417     1  0.5090      0.679 0.728 0.000 0.044 0.228
#&gt; SRR1617418     3  0.0000      0.955 0.000 0.000 1.000 0.000
#&gt; SRR1617419     3  0.0000      0.955 0.000 0.000 1.000 0.000
#&gt; SRR1617420     4  0.0336      0.995 0.008 0.000 0.000 0.992
#&gt; SRR1617421     4  0.0336      0.995 0.008 0.000 0.000 0.992
#&gt; SRR1617422     1  0.2704      0.914 0.876 0.000 0.000 0.124
#&gt; SRR1617423     1  0.2704      0.914 0.876 0.000 0.000 0.124
#&gt; SRR1617424     1  0.2647      0.915 0.880 0.000 0.000 0.120
#&gt; SRR1617425     1  0.2647      0.915 0.880 0.000 0.000 0.120
#&gt; SRR1617427     1  0.2973      0.904 0.856 0.000 0.000 0.144
#&gt; SRR1617426     1  0.2973      0.904 0.856 0.000 0.000 0.144
#&gt; SRR1617428     3  0.6696      0.705 0.088 0.140 0.700 0.072
#&gt; SRR1617429     3  0.6696      0.705 0.088 0.140 0.700 0.072
#&gt; SRR1617432     4  0.0336      0.995 0.008 0.000 0.000 0.992
#&gt; SRR1617433     4  0.0336      0.995 0.008 0.000 0.000 0.992
#&gt; SRR1617434     4  0.0000      0.989 0.000 0.000 0.000 1.000
#&gt; SRR1617436     3  0.0000      0.955 0.000 0.000 1.000 0.000
#&gt; SRR1617435     4  0.0000      0.989 0.000 0.000 0.000 1.000
#&gt; SRR1617437     3  0.0000      0.955 0.000 0.000 1.000 0.000
#&gt; SRR1617438     3  0.0000      0.955 0.000 0.000 1.000 0.000
#&gt; SRR1617439     3  0.0000      0.955 0.000 0.000 1.000 0.000
#&gt; SRR1617440     3  0.0804      0.944 0.012 0.008 0.980 0.000
#&gt; SRR1617441     3  0.0804      0.944 0.012 0.008 0.980 0.000
#&gt; SRR1617443     3  0.0000      0.955 0.000 0.000 1.000 0.000
#&gt; SRR1617442     3  0.0000      0.955 0.000 0.000 1.000 0.000
#&gt; SRR1617444     1  0.0564      0.862 0.988 0.004 0.004 0.004
#&gt; SRR1617445     1  0.0564      0.862 0.988 0.004 0.004 0.004
#&gt; SRR1617446     1  0.2216      0.917 0.908 0.000 0.000 0.092
#&gt; SRR1617447     1  0.2216      0.917 0.908 0.000 0.000 0.092
#&gt; SRR1617448     1  0.2216      0.917 0.908 0.000 0.000 0.092
#&gt; SRR1617449     1  0.2216      0.917 0.908 0.000 0.000 0.092
#&gt; SRR1617451     2  0.0188      0.967 0.004 0.996 0.000 0.000
#&gt; SRR1617450     2  0.0188      0.967 0.004 0.996 0.000 0.000
#&gt; SRR1617452     2  0.4551      0.868 0.116 0.820 0.040 0.024
#&gt; SRR1617454     2  0.0000      0.967 0.000 1.000 0.000 0.000
#&gt; SRR1617453     2  0.4551      0.868 0.116 0.820 0.040 0.024
#&gt; SRR1617456     2  0.1022      0.963 0.032 0.968 0.000 0.000
#&gt; SRR1617457     2  0.1022      0.963 0.032 0.968 0.000 0.000
#&gt; SRR1617455     2  0.0000      0.967 0.000 1.000 0.000 0.000
#&gt; SRR1617458     2  0.1022      0.963 0.032 0.968 0.000 0.000
#&gt; SRR1617459     2  0.1022      0.963 0.032 0.968 0.000 0.000
#&gt; SRR1617460     2  0.1637      0.951 0.060 0.940 0.000 0.000
#&gt; SRR1617461     2  0.1637      0.951 0.060 0.940 0.000 0.000
#&gt; SRR1617463     2  0.0000      0.967 0.000 1.000 0.000 0.000
#&gt; SRR1617462     2  0.0000      0.967 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     2  0.0963      0.853 0.000 0.964 0.000 0.036 0.000
#&gt; SRR1617431     2  0.0963      0.853 0.000 0.964 0.000 0.036 0.000
#&gt; SRR1617410     5  0.0771      0.978 0.004 0.000 0.000 0.020 0.976
#&gt; SRR1617411     5  0.0771      0.978 0.004 0.000 0.000 0.020 0.976
#&gt; SRR1617412     3  0.0000      0.978 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617413     3  0.0000      0.978 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617414     5  0.0510      0.979 0.000 0.000 0.000 0.016 0.984
#&gt; SRR1617415     5  0.0510      0.979 0.000 0.000 0.000 0.016 0.984
#&gt; SRR1617416     4  0.4589      0.739 0.172 0.000 0.008 0.752 0.068
#&gt; SRR1617417     4  0.4589      0.739 0.172 0.000 0.008 0.752 0.068
#&gt; SRR1617418     3  0.0000      0.978 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617419     3  0.0000      0.978 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617420     5  0.0290      0.982 0.000 0.000 0.000 0.008 0.992
#&gt; SRR1617421     5  0.0290      0.982 0.000 0.000 0.000 0.008 0.992
#&gt; SRR1617422     1  0.1195      0.959 0.960 0.000 0.000 0.012 0.028
#&gt; SRR1617423     1  0.1195      0.959 0.960 0.000 0.000 0.012 0.028
#&gt; SRR1617424     1  0.0898      0.961 0.972 0.000 0.000 0.008 0.020
#&gt; SRR1617425     1  0.0898      0.961 0.972 0.000 0.000 0.008 0.020
#&gt; SRR1617427     1  0.1877      0.935 0.924 0.000 0.000 0.012 0.064
#&gt; SRR1617426     1  0.1877      0.935 0.924 0.000 0.000 0.012 0.064
#&gt; SRR1617428     4  0.4277      0.774 0.000 0.100 0.112 0.784 0.004
#&gt; SRR1617429     4  0.4277      0.774 0.000 0.100 0.112 0.784 0.004
#&gt; SRR1617432     5  0.0510      0.979 0.000 0.000 0.000 0.016 0.984
#&gt; SRR1617433     5  0.0510      0.979 0.000 0.000 0.000 0.016 0.984
#&gt; SRR1617434     5  0.0794      0.975 0.000 0.000 0.000 0.028 0.972
#&gt; SRR1617436     3  0.0510      0.969 0.000 0.000 0.984 0.016 0.000
#&gt; SRR1617435     5  0.0794      0.975 0.000 0.000 0.000 0.028 0.972
#&gt; SRR1617437     3  0.0510      0.969 0.000 0.000 0.984 0.016 0.000
#&gt; SRR1617438     3  0.0000      0.978 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617439     3  0.0000      0.978 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617440     3  0.2450      0.898 0.000 0.048 0.900 0.052 0.000
#&gt; SRR1617441     3  0.2450      0.898 0.000 0.048 0.900 0.052 0.000
#&gt; SRR1617443     3  0.0000      0.978 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617442     3  0.0000      0.978 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617444     1  0.1341      0.933 0.944 0.000 0.000 0.056 0.000
#&gt; SRR1617445     1  0.1341      0.933 0.944 0.000 0.000 0.056 0.000
#&gt; SRR1617446     1  0.1300      0.959 0.956 0.000 0.000 0.028 0.016
#&gt; SRR1617447     1  0.1300      0.959 0.956 0.000 0.000 0.028 0.016
#&gt; SRR1617448     1  0.1195      0.959 0.960 0.000 0.000 0.028 0.012
#&gt; SRR1617449     1  0.1195      0.959 0.960 0.000 0.000 0.028 0.012
#&gt; SRR1617451     2  0.0510      0.861 0.000 0.984 0.000 0.016 0.000
#&gt; SRR1617450     2  0.0510      0.861 0.000 0.984 0.000 0.016 0.000
#&gt; SRR1617452     4  0.2411      0.726 0.000 0.108 0.000 0.884 0.008
#&gt; SRR1617454     2  0.0290      0.864 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1617453     4  0.2411      0.726 0.000 0.108 0.000 0.884 0.008
#&gt; SRR1617456     2  0.3039      0.839 0.012 0.836 0.000 0.152 0.000
#&gt; SRR1617457     2  0.3039      0.839 0.012 0.836 0.000 0.152 0.000
#&gt; SRR1617455     2  0.0290      0.864 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1617458     2  0.3081      0.838 0.012 0.832 0.000 0.156 0.000
#&gt; SRR1617459     2  0.3081      0.838 0.012 0.832 0.000 0.156 0.000
#&gt; SRR1617460     2  0.4384      0.677 0.016 0.660 0.000 0.324 0.000
#&gt; SRR1617461     2  0.4384      0.677 0.016 0.660 0.000 0.324 0.000
#&gt; SRR1617463     2  0.2629      0.824 0.004 0.860 0.000 0.136 0.000
#&gt; SRR1617462     2  0.2629      0.824 0.004 0.860 0.000 0.136 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.4010      0.727 0.000 0.584 0.000 0.008 0.000 0.408
#&gt; SRR1617431     2  0.4010      0.727 0.000 0.584 0.000 0.008 0.000 0.408
#&gt; SRR1617410     5  0.0363      0.937 0.000 0.000 0.000 0.012 0.988 0.000
#&gt; SRR1617411     5  0.0363      0.937 0.000 0.000 0.000 0.012 0.988 0.000
#&gt; SRR1617412     3  0.0820      0.921 0.000 0.000 0.972 0.012 0.000 0.016
#&gt; SRR1617413     3  0.0820      0.921 0.000 0.000 0.972 0.012 0.000 0.016
#&gt; SRR1617414     5  0.2871      0.909 0.008 0.000 0.000 0.024 0.852 0.116
#&gt; SRR1617415     5  0.2871      0.909 0.008 0.000 0.000 0.024 0.852 0.116
#&gt; SRR1617416     4  0.2017      0.893 0.020 0.000 0.004 0.920 0.048 0.008
#&gt; SRR1617417     4  0.2017      0.893 0.020 0.000 0.004 0.920 0.048 0.008
#&gt; SRR1617418     3  0.0520      0.923 0.000 0.000 0.984 0.008 0.000 0.008
#&gt; SRR1617419     3  0.0520      0.923 0.000 0.000 0.984 0.008 0.000 0.008
#&gt; SRR1617420     5  0.0000      0.938 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617421     5  0.0000      0.938 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617422     1  0.1429      0.887 0.940 0.000 0.000 0.004 0.004 0.052
#&gt; SRR1617423     1  0.1429      0.887 0.940 0.000 0.000 0.004 0.004 0.052
#&gt; SRR1617424     1  0.0146      0.894 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; SRR1617425     1  0.0146      0.894 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; SRR1617427     1  0.1624      0.875 0.936 0.000 0.000 0.004 0.040 0.020
#&gt; SRR1617426     1  0.1624      0.875 0.936 0.000 0.000 0.004 0.040 0.020
#&gt; SRR1617428     4  0.2214      0.884 0.000 0.004 0.012 0.892 0.000 0.092
#&gt; SRR1617429     4  0.2214      0.884 0.000 0.004 0.012 0.892 0.000 0.092
#&gt; SRR1617432     5  0.2760      0.911 0.004 0.000 0.000 0.024 0.856 0.116
#&gt; SRR1617433     5  0.2760      0.911 0.004 0.000 0.000 0.024 0.856 0.116
#&gt; SRR1617434     5  0.0363      0.937 0.000 0.000 0.000 0.012 0.988 0.000
#&gt; SRR1617436     3  0.1563      0.900 0.000 0.000 0.932 0.012 0.000 0.056
#&gt; SRR1617435     5  0.0363      0.937 0.000 0.000 0.000 0.012 0.988 0.000
#&gt; SRR1617437     3  0.1563      0.900 0.000 0.000 0.932 0.012 0.000 0.056
#&gt; SRR1617438     3  0.1007      0.922 0.000 0.000 0.956 0.000 0.000 0.044
#&gt; SRR1617439     3  0.1007      0.922 0.000 0.000 0.956 0.000 0.000 0.044
#&gt; SRR1617440     3  0.4459      0.739 0.000 0.204 0.708 0.004 0.000 0.084
#&gt; SRR1617441     3  0.4459      0.739 0.000 0.204 0.708 0.004 0.000 0.084
#&gt; SRR1617443     3  0.0937      0.923 0.000 0.000 0.960 0.000 0.000 0.040
#&gt; SRR1617442     3  0.0937      0.923 0.000 0.000 0.960 0.000 0.000 0.040
#&gt; SRR1617444     1  0.4173      0.840 0.732 0.012 0.000 0.044 0.000 0.212
#&gt; SRR1617445     1  0.4173      0.840 0.732 0.012 0.000 0.044 0.000 0.212
#&gt; SRR1617446     1  0.2831      0.891 0.840 0.000 0.000 0.024 0.000 0.136
#&gt; SRR1617447     1  0.2831      0.891 0.840 0.000 0.000 0.024 0.000 0.136
#&gt; SRR1617448     1  0.2831      0.891 0.840 0.000 0.000 0.024 0.000 0.136
#&gt; SRR1617449     1  0.2831      0.891 0.840 0.000 0.000 0.024 0.000 0.136
#&gt; SRR1617451     2  0.3847      0.745 0.000 0.644 0.000 0.008 0.000 0.348
#&gt; SRR1617450     2  0.3847      0.745 0.000 0.644 0.000 0.008 0.000 0.348
#&gt; SRR1617452     4  0.2408      0.875 0.000 0.108 0.004 0.876 0.000 0.012
#&gt; SRR1617454     2  0.3684      0.750 0.000 0.628 0.000 0.000 0.000 0.372
#&gt; SRR1617453     4  0.2408      0.875 0.000 0.108 0.004 0.876 0.000 0.012
#&gt; SRR1617456     2  0.0000      0.703 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617457     2  0.0000      0.703 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617455     2  0.3684      0.750 0.000 0.628 0.000 0.000 0.000 0.372
#&gt; SRR1617458     2  0.0000      0.703 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617459     2  0.0000      0.703 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617460     2  0.5660      0.518 0.000 0.516 0.000 0.184 0.000 0.300
#&gt; SRR1617461     2  0.5660      0.518 0.000 0.516 0.000 0.184 0.000 0.300
#&gt; SRR1617463     2  0.5181      0.637 0.000 0.484 0.000 0.088 0.000 0.428
#&gt; SRR1617462     2  0.5181      0.637 0.000 0.484 0.000 0.088 0.000 0.428
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:pam






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.478           0.895       0.897         0.4250 0.575   0.575
#> 3 3 0.719           0.831       0.885         0.4410 0.782   0.621
#> 4 4 0.826           0.876       0.914         0.1369 0.966   0.906
#> 5 5 0.798           0.832       0.880         0.0870 0.916   0.741
#> 6 6 0.861           0.845       0.919         0.0457 0.958   0.827
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2   0.000      1.000 0.000 1.000
#&gt; SRR1617431     2   0.000      1.000 0.000 1.000
#&gt; SRR1617410     1   0.000      0.833 1.000 0.000
#&gt; SRR1617411     1   0.000      0.833 1.000 0.000
#&gt; SRR1617412     1   0.714      0.891 0.804 0.196
#&gt; SRR1617413     1   0.714      0.891 0.804 0.196
#&gt; SRR1617414     1   0.000      0.833 1.000 0.000
#&gt; SRR1617415     1   0.000      0.833 1.000 0.000
#&gt; SRR1617416     1   0.000      0.833 1.000 0.000
#&gt; SRR1617417     1   0.000      0.833 1.000 0.000
#&gt; SRR1617418     1   0.722      0.890 0.800 0.200
#&gt; SRR1617419     1   0.722      0.890 0.800 0.200
#&gt; SRR1617420     1   0.000      0.833 1.000 0.000
#&gt; SRR1617421     1   0.000      0.833 1.000 0.000
#&gt; SRR1617422     1   0.697      0.894 0.812 0.188
#&gt; SRR1617423     1   0.697      0.894 0.812 0.188
#&gt; SRR1617424     1   0.697      0.894 0.812 0.188
#&gt; SRR1617425     1   0.697      0.894 0.812 0.188
#&gt; SRR1617427     1   0.689      0.894 0.816 0.184
#&gt; SRR1617426     1   0.689      0.894 0.816 0.184
#&gt; SRR1617428     1   0.850      0.759 0.724 0.276
#&gt; SRR1617429     1   0.900      0.698 0.684 0.316
#&gt; SRR1617432     1   0.000      0.833 1.000 0.000
#&gt; SRR1617433     1   0.000      0.833 1.000 0.000
#&gt; SRR1617434     1   0.000      0.833 1.000 0.000
#&gt; SRR1617436     1   0.722      0.890 0.800 0.200
#&gt; SRR1617435     1   0.000      0.833 1.000 0.000
#&gt; SRR1617437     1   0.722      0.890 0.800 0.200
#&gt; SRR1617438     1   0.722      0.890 0.800 0.200
#&gt; SRR1617439     1   0.722      0.890 0.800 0.200
#&gt; SRR1617440     1   0.971      0.629 0.600 0.400
#&gt; SRR1617441     1   0.971      0.629 0.600 0.400
#&gt; SRR1617443     1   0.680      0.891 0.820 0.180
#&gt; SRR1617442     1   0.671      0.891 0.824 0.176
#&gt; SRR1617444     1   0.697      0.894 0.812 0.188
#&gt; SRR1617445     1   0.697      0.894 0.812 0.188
#&gt; SRR1617446     1   0.697      0.894 0.812 0.188
#&gt; SRR1617447     1   0.697      0.894 0.812 0.188
#&gt; SRR1617448     1   0.697      0.894 0.812 0.188
#&gt; SRR1617449     1   0.697      0.894 0.812 0.188
#&gt; SRR1617451     2   0.000      1.000 0.000 1.000
#&gt; SRR1617450     2   0.000      1.000 0.000 1.000
#&gt; SRR1617452     2   0.000      1.000 0.000 1.000
#&gt; SRR1617454     2   0.000      1.000 0.000 1.000
#&gt; SRR1617453     2   0.000      1.000 0.000 1.000
#&gt; SRR1617456     2   0.000      1.000 0.000 1.000
#&gt; SRR1617457     2   0.000      1.000 0.000 1.000
#&gt; SRR1617455     2   0.000      1.000 0.000 1.000
#&gt; SRR1617458     2   0.000      1.000 0.000 1.000
#&gt; SRR1617459     2   0.000      1.000 0.000 1.000
#&gt; SRR1617460     2   0.000      1.000 0.000 1.000
#&gt; SRR1617461     2   0.000      1.000 0.000 1.000
#&gt; SRR1617463     2   0.000      1.000 0.000 1.000
#&gt; SRR1617462     2   0.000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2   0.000      0.886 0.000 1.000 0.000
#&gt; SRR1617431     2   0.000      0.886 0.000 1.000 0.000
#&gt; SRR1617410     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617411     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617412     3   0.514      0.926 0.252 0.000 0.748
#&gt; SRR1617413     3   0.514      0.926 0.252 0.000 0.748
#&gt; SRR1617414     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617415     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617416     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617417     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617418     3   0.514      0.926 0.252 0.000 0.748
#&gt; SRR1617419     3   0.514      0.926 0.252 0.000 0.748
#&gt; SRR1617420     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617421     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617422     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617423     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617424     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617425     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617427     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617426     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617428     1   0.725     -0.198 0.536 0.028 0.436
#&gt; SRR1617429     1   0.714     -0.197 0.540 0.024 0.436
#&gt; SRR1617432     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617433     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617434     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617436     3   0.518      0.921 0.256 0.000 0.744
#&gt; SRR1617435     1   0.000      0.905 1.000 0.000 0.000
#&gt; SRR1617437     3   0.518      0.921 0.256 0.000 0.744
#&gt; SRR1617438     3   0.514      0.926 0.252 0.000 0.748
#&gt; SRR1617439     3   0.514      0.926 0.252 0.000 0.748
#&gt; SRR1617440     3   0.000      0.627 0.000 0.000 1.000
#&gt; SRR1617441     3   0.000      0.627 0.000 0.000 1.000
#&gt; SRR1617443     3   0.514      0.926 0.252 0.000 0.748
#&gt; SRR1617442     3   0.514      0.926 0.252 0.000 0.748
#&gt; SRR1617444     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617445     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617446     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617447     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617448     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617449     1   0.293      0.905 0.924 0.036 0.040
#&gt; SRR1617451     2   0.000      0.886 0.000 1.000 0.000
#&gt; SRR1617450     2   0.000      0.886 0.000 1.000 0.000
#&gt; SRR1617452     2   0.595      0.522 0.000 0.640 0.360
#&gt; SRR1617454     2   0.000      0.886 0.000 1.000 0.000
#&gt; SRR1617453     2   0.595      0.527 0.000 0.640 0.360
#&gt; SRR1617456     2   0.514      0.798 0.000 0.748 0.252
#&gt; SRR1617457     2   0.514      0.798 0.000 0.748 0.252
#&gt; SRR1617455     2   0.000      0.886 0.000 1.000 0.000
#&gt; SRR1617458     2   0.514      0.798 0.000 0.748 0.252
#&gt; SRR1617459     2   0.514      0.798 0.000 0.748 0.252
#&gt; SRR1617460     2   0.255      0.853 0.040 0.936 0.024
#&gt; SRR1617461     2   0.255      0.853 0.040 0.936 0.024
#&gt; SRR1617463     2   0.000      0.886 0.000 1.000 0.000
#&gt; SRR1617462     2   0.000      0.886 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.0707      0.870 0.000 0.980 0.020 0.000
#&gt; SRR1617431     2  0.0592      0.872 0.000 0.984 0.016 0.000
#&gt; SRR1617410     1  0.0000      0.868 1.000 0.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.868 1.000 0.000 0.000 0.000
#&gt; SRR1617412     3  0.0469      0.985 0.012 0.000 0.988 0.000
#&gt; SRR1617413     3  0.0469      0.985 0.012 0.000 0.988 0.000
#&gt; SRR1617414     1  0.0336      0.866 0.992 0.000 0.000 0.008
#&gt; SRR1617415     1  0.0336      0.866 0.992 0.000 0.000 0.008
#&gt; SRR1617416     1  0.3172      0.890 0.840 0.000 0.000 0.160
#&gt; SRR1617417     1  0.2704      0.887 0.876 0.000 0.000 0.124
#&gt; SRR1617418     3  0.0000      0.993 0.000 0.000 1.000 0.000
#&gt; SRR1617419     3  0.0000      0.993 0.000 0.000 1.000 0.000
#&gt; SRR1617420     1  0.0188      0.867 0.996 0.000 0.000 0.004
#&gt; SRR1617421     1  0.0188      0.867 0.996 0.000 0.000 0.004
#&gt; SRR1617422     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617423     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617424     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617425     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617427     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617426     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617428     1  0.5070      0.338 0.580 0.004 0.416 0.000
#&gt; SRR1617429     1  0.5070      0.338 0.580 0.004 0.416 0.000
#&gt; SRR1617432     1  0.0336      0.866 0.992 0.000 0.000 0.008
#&gt; SRR1617433     1  0.0336      0.866 0.992 0.000 0.000 0.008
#&gt; SRR1617434     1  0.0336      0.866 0.992 0.000 0.000 0.008
#&gt; SRR1617436     3  0.0469      0.985 0.012 0.000 0.988 0.000
#&gt; SRR1617435     1  0.0336      0.866 0.992 0.000 0.000 0.008
#&gt; SRR1617437     3  0.0469      0.985 0.012 0.000 0.988 0.000
#&gt; SRR1617438     3  0.0000      0.993 0.000 0.000 1.000 0.000
#&gt; SRR1617439     3  0.0000      0.993 0.000 0.000 1.000 0.000
#&gt; SRR1617440     3  0.0000      0.993 0.000 0.000 1.000 0.000
#&gt; SRR1617441     3  0.0000      0.993 0.000 0.000 1.000 0.000
#&gt; SRR1617443     3  0.0000      0.993 0.000 0.000 1.000 0.000
#&gt; SRR1617442     3  0.0000      0.993 0.000 0.000 1.000 0.000
#&gt; SRR1617444     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617445     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617446     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617447     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617448     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617449     1  0.3356      0.891 0.824 0.000 0.000 0.176
#&gt; SRR1617451     2  0.3356      0.724 0.000 0.824 0.000 0.176
#&gt; SRR1617450     2  0.3400      0.719 0.000 0.820 0.000 0.180
#&gt; SRR1617452     2  0.5466      0.577 0.000 0.712 0.220 0.068
#&gt; SRR1617454     2  0.0000      0.879 0.000 1.000 0.000 0.000
#&gt; SRR1617453     2  0.5533      0.572 0.000 0.708 0.220 0.072
#&gt; SRR1617456     4  0.3444      1.000 0.000 0.184 0.000 0.816
#&gt; SRR1617457     4  0.3444      1.000 0.000 0.184 0.000 0.816
#&gt; SRR1617455     2  0.0000      0.879 0.000 1.000 0.000 0.000
#&gt; SRR1617458     4  0.3444      1.000 0.000 0.184 0.000 0.816
#&gt; SRR1617459     4  0.3444      1.000 0.000 0.184 0.000 0.816
#&gt; SRR1617460     2  0.0000      0.879 0.000 1.000 0.000 0.000
#&gt; SRR1617461     2  0.0000      0.879 0.000 1.000 0.000 0.000
#&gt; SRR1617463     2  0.0000      0.879 0.000 1.000 0.000 0.000
#&gt; SRR1617462     2  0.0000      0.879 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     2  0.5862      0.652 0.000 0.604 0.176 0.000 0.220
#&gt; SRR1617431     2  0.5831      0.654 0.000 0.608 0.172 0.000 0.220
#&gt; SRR1617410     1  0.3039      0.699 0.808 0.000 0.000 0.000 0.192
#&gt; SRR1617411     1  0.2966      0.709 0.816 0.000 0.000 0.000 0.184
#&gt; SRR1617412     3  0.0404      0.986 0.012 0.000 0.988 0.000 0.000
#&gt; SRR1617413     3  0.0404      0.986 0.012 0.000 0.988 0.000 0.000
#&gt; SRR1617414     5  0.3274      1.000 0.220 0.000 0.000 0.000 0.780
#&gt; SRR1617415     5  0.3274      1.000 0.220 0.000 0.000 0.000 0.780
#&gt; SRR1617416     1  0.0510      0.863 0.984 0.000 0.000 0.000 0.016
#&gt; SRR1617417     1  0.1341      0.835 0.944 0.000 0.000 0.000 0.056
#&gt; SRR1617418     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617419     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617420     1  0.3109      0.687 0.800 0.000 0.000 0.000 0.200
#&gt; SRR1617421     1  0.3143      0.681 0.796 0.000 0.000 0.000 0.204
#&gt; SRR1617422     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617428     1  0.5338      0.270 0.544 0.000 0.400 0.000 0.056
#&gt; SRR1617429     1  0.5338      0.270 0.544 0.000 0.400 0.000 0.056
#&gt; SRR1617432     5  0.3274      1.000 0.220 0.000 0.000 0.000 0.780
#&gt; SRR1617433     5  0.3274      1.000 0.220 0.000 0.000 0.000 0.780
#&gt; SRR1617434     5  0.3274      1.000 0.220 0.000 0.000 0.000 0.780
#&gt; SRR1617436     3  0.0404      0.986 0.012 0.000 0.988 0.000 0.000
#&gt; SRR1617435     5  0.3274      1.000 0.220 0.000 0.000 0.000 0.780
#&gt; SRR1617437     3  0.0404      0.986 0.012 0.000 0.988 0.000 0.000
#&gt; SRR1617438     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617439     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617440     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617441     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617443     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617442     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617444     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617445     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617446     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.872 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617451     2  0.6423      0.550 0.000 0.504 0.000 0.276 0.220
#&gt; SRR1617450     2  0.6438      0.545 0.000 0.500 0.000 0.280 0.220
#&gt; SRR1617452     2  0.6031      0.418 0.000 0.576 0.180 0.244 0.000
#&gt; SRR1617454     2  0.3274      0.711 0.000 0.780 0.000 0.000 0.220
#&gt; SRR1617453     2  0.6080      0.406 0.000 0.568 0.184 0.248 0.000
#&gt; SRR1617456     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617457     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617455     2  0.3274      0.711 0.000 0.780 0.000 0.000 0.220
#&gt; SRR1617458     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617459     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617460     2  0.0000      0.725 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617461     2  0.0000      0.725 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617463     2  0.0000      0.725 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617462     2  0.0000      0.725 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.2805      0.625 0.000 0.812 0.184 0.004 0.000 0.000
#&gt; SRR1617431     2  0.2772      0.629 0.000 0.816 0.180 0.004 0.000 0.000
#&gt; SRR1617410     1  0.2730      0.766 0.808 0.000 0.000 0.000 0.192 0.000
#&gt; SRR1617411     1  0.2664      0.775 0.816 0.000 0.000 0.000 0.184 0.000
#&gt; SRR1617412     3  0.0363      0.985 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; SRR1617413     3  0.0363      0.985 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; SRR1617414     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617415     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617416     1  0.0363      0.927 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; SRR1617417     1  0.1007      0.905 0.956 0.000 0.000 0.000 0.044 0.000
#&gt; SRR1617418     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617419     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617420     1  0.2793      0.756 0.800 0.000 0.000 0.000 0.200 0.000
#&gt; SRR1617421     1  0.2823      0.751 0.796 0.000 0.000 0.000 0.204 0.000
#&gt; SRR1617422     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617428     4  0.4727      0.360 0.368 0.000 0.056 0.576 0.000 0.000
#&gt; SRR1617429     4  0.4727      0.360 0.368 0.000 0.056 0.576 0.000 0.000
#&gt; SRR1617432     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617433     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617434     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617436     3  0.0363      0.985 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; SRR1617435     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617437     3  0.0363      0.985 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; SRR1617438     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617439     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617440     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617441     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617443     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617442     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617444     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617445     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617446     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.934 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617451     2  0.3337      0.576 0.000 0.736 0.000 0.004 0.000 0.260
#&gt; SRR1617450     2  0.3360      0.572 0.000 0.732 0.000 0.004 0.000 0.264
#&gt; SRR1617452     4  0.5748      0.138 0.000 0.176 0.032 0.608 0.000 0.184
#&gt; SRR1617454     2  0.0000      0.726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617453     4  0.5785      0.144 0.000 0.172 0.036 0.608 0.000 0.184
#&gt; SRR1617456     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617457     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617455     2  0.0000      0.726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617458     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617459     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617460     2  0.3023      0.705 0.000 0.768 0.000 0.232 0.000 0.000
#&gt; SRR1617461     2  0.3023      0.705 0.000 0.768 0.000 0.232 0.000 0.000
#&gt; SRR1617463     2  0.3023      0.705 0.000 0.768 0.000 0.232 0.000 0.000
#&gt; SRR1617462     2  0.3023      0.705 0.000 0.768 0.000 0.232 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.625           0.888       0.943         0.4726 0.525   0.525
#> 3 3 0.969           0.942       0.973         0.3686 0.832   0.680
#> 4 4 0.827           0.796       0.894         0.1249 0.871   0.653
#> 5 5 0.864           0.815       0.859         0.0450 0.941   0.794
#> 6 6 0.807           0.735       0.809         0.0513 0.966   0.871
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617431     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617410     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617411     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617412     2  0.0000      0.923 0.000 1.000
#&gt; SRR1617413     2  0.0000      0.923 0.000 1.000
#&gt; SRR1617414     1  0.7139      0.767 0.804 0.196
#&gt; SRR1617415     1  0.7139      0.767 0.804 0.196
#&gt; SRR1617416     2  0.8327      0.702 0.264 0.736
#&gt; SRR1617417     2  0.8327      0.702 0.264 0.736
#&gt; SRR1617418     2  0.0000      0.923 0.000 1.000
#&gt; SRR1617419     2  0.0000      0.923 0.000 1.000
#&gt; SRR1617420     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617421     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617422     1  0.6801      0.789 0.820 0.180
#&gt; SRR1617423     1  0.6887      0.784 0.816 0.184
#&gt; SRR1617424     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617425     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617427     1  0.1414      0.941 0.980 0.020
#&gt; SRR1617426     1  0.1414      0.941 0.980 0.020
#&gt; SRR1617428     2  0.8327      0.702 0.264 0.736
#&gt; SRR1617429     2  0.8327      0.702 0.264 0.736
#&gt; SRR1617432     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617433     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617434     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617436     2  0.0000      0.923 0.000 1.000
#&gt; SRR1617435     1  0.0672      0.948 0.992 0.008
#&gt; SRR1617437     2  0.0000      0.923 0.000 1.000
#&gt; SRR1617438     2  0.0000      0.923 0.000 1.000
#&gt; SRR1617439     2  0.0000      0.923 0.000 1.000
#&gt; SRR1617440     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617441     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617443     2  0.0000      0.923 0.000 1.000
#&gt; SRR1617442     2  0.0000      0.923 0.000 1.000
#&gt; SRR1617444     2  0.8327      0.702 0.264 0.736
#&gt; SRR1617445     2  0.8327      0.702 0.264 0.736
#&gt; SRR1617446     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617447     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617448     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617449     1  0.0000      0.951 1.000 0.000
#&gt; SRR1617451     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617450     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617452     2  0.8327      0.702 0.264 0.736
#&gt; SRR1617454     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617453     2  0.8327      0.702 0.264 0.736
#&gt; SRR1617456     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617457     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617455     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617458     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617459     2  0.0672      0.926 0.008 0.992
#&gt; SRR1617460     2  0.0938      0.924 0.012 0.988
#&gt; SRR1617461     2  0.0938      0.924 0.012 0.988
#&gt; SRR1617463     2  0.0938      0.924 0.012 0.988
#&gt; SRR1617462     2  0.0938      0.924 0.012 0.988
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.0892      0.953 0.000 0.980 0.020
#&gt; SRR1617431     2  0.0892      0.953 0.000 0.980 0.020
#&gt; SRR1617410     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617412     3  0.0000      0.990 0.000 0.000 1.000
#&gt; SRR1617413     3  0.0000      0.990 0.000 0.000 1.000
#&gt; SRR1617414     1  0.4121      0.788 0.832 0.168 0.000
#&gt; SRR1617415     1  0.4178      0.783 0.828 0.172 0.000
#&gt; SRR1617416     2  0.1315      0.949 0.020 0.972 0.008
#&gt; SRR1617417     2  0.1315      0.949 0.020 0.972 0.008
#&gt; SRR1617418     3  0.0000      0.990 0.000 0.000 1.000
#&gt; SRR1617419     3  0.0000      0.990 0.000 0.000 1.000
#&gt; SRR1617420     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617421     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617422     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617428     2  0.0848      0.956 0.008 0.984 0.008
#&gt; SRR1617429     2  0.0848      0.956 0.008 0.984 0.008
#&gt; SRR1617432     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617433     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617434     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617436     3  0.1529      0.960 0.000 0.040 0.960
#&gt; SRR1617435     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617437     3  0.1529      0.960 0.000 0.040 0.960
#&gt; SRR1617438     3  0.0000      0.990 0.000 0.000 1.000
#&gt; SRR1617439     3  0.0000      0.990 0.000 0.000 1.000
#&gt; SRR1617440     2  0.6045      0.424 0.000 0.620 0.380
#&gt; SRR1617441     2  0.6045      0.424 0.000 0.620 0.380
#&gt; SRR1617443     3  0.0000      0.990 0.000 0.000 1.000
#&gt; SRR1617442     3  0.0000      0.990 0.000 0.000 1.000
#&gt; SRR1617444     2  0.1585      0.943 0.028 0.964 0.008
#&gt; SRR1617445     2  0.1585      0.943 0.028 0.964 0.008
#&gt; SRR1617446     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.978 1.000 0.000 0.000
#&gt; SRR1617451     2  0.0000      0.955 0.000 1.000 0.000
#&gt; SRR1617450     2  0.0000      0.955 0.000 1.000 0.000
#&gt; SRR1617452     2  0.0848      0.956 0.008 0.984 0.008
#&gt; SRR1617454     2  0.0000      0.955 0.000 1.000 0.000
#&gt; SRR1617453     2  0.0848      0.956 0.008 0.984 0.008
#&gt; SRR1617456     2  0.0237      0.955 0.000 0.996 0.004
#&gt; SRR1617457     2  0.0237      0.955 0.000 0.996 0.004
#&gt; SRR1617455     2  0.0000      0.955 0.000 1.000 0.000
#&gt; SRR1617458     2  0.0000      0.955 0.000 1.000 0.000
#&gt; SRR1617459     2  0.0000      0.955 0.000 1.000 0.000
#&gt; SRR1617460     2  0.0424      0.956 0.000 0.992 0.008
#&gt; SRR1617461     2  0.0424      0.956 0.000 0.992 0.008
#&gt; SRR1617463     2  0.0424      0.956 0.000 0.992 0.008
#&gt; SRR1617462     2  0.0424      0.956 0.000 0.992 0.008
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.3448      0.643 0.000 0.828 0.004 0.168
#&gt; SRR1617431     2  0.3448      0.643 0.000 0.828 0.004 0.168
#&gt; SRR1617410     1  0.0188      0.988 0.996 0.000 0.004 0.000
#&gt; SRR1617411     1  0.0188      0.988 0.996 0.000 0.004 0.000
#&gt; SRR1617412     3  0.0000      0.905 0.000 0.000 1.000 0.000
#&gt; SRR1617413     3  0.0000      0.905 0.000 0.000 1.000 0.000
#&gt; SRR1617414     1  0.1798      0.933 0.944 0.016 0.000 0.040
#&gt; SRR1617415     1  0.1798      0.933 0.944 0.016 0.000 0.040
#&gt; SRR1617416     4  0.4817      0.621 0.088 0.128 0.000 0.784
#&gt; SRR1617417     4  0.4817      0.621 0.088 0.128 0.000 0.784
#&gt; SRR1617418     3  0.0000      0.905 0.000 0.000 1.000 0.000
#&gt; SRR1617419     3  0.0000      0.905 0.000 0.000 1.000 0.000
#&gt; SRR1617420     1  0.0188      0.988 0.996 0.000 0.004 0.000
#&gt; SRR1617421     1  0.0188      0.988 0.996 0.000 0.004 0.000
#&gt; SRR1617422     1  0.0564      0.986 0.988 0.004 0.004 0.004
#&gt; SRR1617423     1  0.0524      0.983 0.988 0.004 0.000 0.008
#&gt; SRR1617424     1  0.0376      0.987 0.992 0.004 0.004 0.000
#&gt; SRR1617425     1  0.0376      0.987 0.992 0.004 0.004 0.000
#&gt; SRR1617427     1  0.0188      0.987 0.996 0.004 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.987 1.000 0.000 0.000 0.000
#&gt; SRR1617428     4  0.4879      0.620 0.092 0.128 0.000 0.780
#&gt; SRR1617429     4  0.4879      0.620 0.092 0.128 0.000 0.780
#&gt; SRR1617432     1  0.0524      0.985 0.988 0.000 0.004 0.008
#&gt; SRR1617433     1  0.0524      0.985 0.988 0.000 0.004 0.008
#&gt; SRR1617434     1  0.0336      0.984 0.992 0.000 0.000 0.008
#&gt; SRR1617436     3  0.0000      0.905 0.000 0.000 1.000 0.000
#&gt; SRR1617435     1  0.0336      0.984 0.992 0.000 0.000 0.008
#&gt; SRR1617437     3  0.0000      0.905 0.000 0.000 1.000 0.000
#&gt; SRR1617438     3  0.0000      0.905 0.000 0.000 1.000 0.000
#&gt; SRR1617439     3  0.0000      0.905 0.000 0.000 1.000 0.000
#&gt; SRR1617440     3  0.7138      0.253 0.000 0.296 0.540 0.164
#&gt; SRR1617441     3  0.7138      0.253 0.000 0.296 0.540 0.164
#&gt; SRR1617443     3  0.0000      0.905 0.000 0.000 1.000 0.000
#&gt; SRR1617442     3  0.0000      0.905 0.000 0.000 1.000 0.000
#&gt; SRR1617444     4  0.8012      0.373 0.268 0.360 0.004 0.368
#&gt; SRR1617445     4  0.8012      0.373 0.268 0.360 0.004 0.368
#&gt; SRR1617446     1  0.0188      0.987 0.996 0.004 0.000 0.000
#&gt; SRR1617447     1  0.0188      0.987 0.996 0.004 0.000 0.000
#&gt; SRR1617448     1  0.0376      0.987 0.992 0.004 0.004 0.000
#&gt; SRR1617449     1  0.0376      0.987 0.992 0.004 0.004 0.000
#&gt; SRR1617451     2  0.0592      0.829 0.000 0.984 0.000 0.016
#&gt; SRR1617450     2  0.0592      0.829 0.000 0.984 0.000 0.016
#&gt; SRR1617452     4  0.1022      0.588 0.000 0.032 0.000 0.968
#&gt; SRR1617454     2  0.0336      0.833 0.000 0.992 0.000 0.008
#&gt; SRR1617453     4  0.1022      0.588 0.000 0.032 0.000 0.968
#&gt; SRR1617456     2  0.2921      0.815 0.000 0.860 0.000 0.140
#&gt; SRR1617457     2  0.2921      0.815 0.000 0.860 0.000 0.140
#&gt; SRR1617455     2  0.0336      0.833 0.000 0.992 0.000 0.008
#&gt; SRR1617458     2  0.2921      0.815 0.000 0.860 0.000 0.140
#&gt; SRR1617459     2  0.2921      0.815 0.000 0.860 0.000 0.140
#&gt; SRR1617460     4  0.4643      0.385 0.000 0.344 0.000 0.656
#&gt; SRR1617461     4  0.4643      0.385 0.000 0.344 0.000 0.656
#&gt; SRR1617463     4  0.4679      0.372 0.000 0.352 0.000 0.648
#&gt; SRR1617462     4  0.4679      0.372 0.000 0.352 0.000 0.648
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     2  0.0510     0.8469 0.000 0.984 0.000 0.000 0.016
#&gt; SRR1617431     2  0.0510     0.8469 0.000 0.984 0.000 0.000 0.016
#&gt; SRR1617410     1  0.1012     0.8985 0.968 0.000 0.000 0.012 0.020
#&gt; SRR1617411     1  0.1012     0.8985 0.968 0.000 0.000 0.012 0.020
#&gt; SRR1617412     3  0.0000     0.8881 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617413     3  0.0000     0.8881 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617414     1  0.3898     0.8112 0.832 0.084 0.000 0.044 0.040
#&gt; SRR1617415     1  0.3898     0.8112 0.832 0.084 0.000 0.044 0.040
#&gt; SRR1617416     4  0.1872     0.8893 0.020 0.000 0.000 0.928 0.052
#&gt; SRR1617417     4  0.1872     0.8893 0.020 0.000 0.000 0.928 0.052
#&gt; SRR1617418     3  0.0000     0.8881 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617419     3  0.0000     0.8881 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617420     1  0.1399     0.8933 0.952 0.000 0.000 0.020 0.028
#&gt; SRR1617421     1  0.1399     0.8933 0.952 0.000 0.000 0.020 0.028
#&gt; SRR1617422     1  0.0898     0.8984 0.972 0.000 0.000 0.008 0.020
#&gt; SRR1617423     1  0.0898     0.8984 0.972 0.000 0.000 0.008 0.020
#&gt; SRR1617424     1  0.0000     0.9007 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000     0.9007 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.2221     0.8774 0.912 0.000 0.000 0.036 0.052
#&gt; SRR1617426     1  0.2221     0.8774 0.912 0.000 0.000 0.036 0.052
#&gt; SRR1617428     4  0.1743     0.8847 0.028 0.004 0.000 0.940 0.028
#&gt; SRR1617429     4  0.1743     0.8847 0.028 0.004 0.000 0.940 0.028
#&gt; SRR1617432     1  0.1549     0.8922 0.944 0.000 0.000 0.016 0.040
#&gt; SRR1617433     1  0.1549     0.8922 0.944 0.000 0.000 0.016 0.040
#&gt; SRR1617434     1  0.1444     0.8925 0.948 0.000 0.000 0.012 0.040
#&gt; SRR1617436     3  0.0404     0.8822 0.000 0.000 0.988 0.000 0.012
#&gt; SRR1617435     1  0.1444     0.8925 0.948 0.000 0.000 0.012 0.040
#&gt; SRR1617437     3  0.0404     0.8822 0.000 0.000 0.988 0.000 0.012
#&gt; SRR1617438     3  0.0000     0.8881 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617439     3  0.0000     0.8881 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617440     3  0.5114    -0.0202 0.000 0.476 0.488 0.000 0.036
#&gt; SRR1617441     3  0.5114    -0.0202 0.000 0.476 0.488 0.000 0.036
#&gt; SRR1617443     3  0.0000     0.8881 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617442     3  0.0000     0.8881 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617444     1  0.7478    -0.0473 0.400 0.220 0.000 0.336 0.044
#&gt; SRR1617445     1  0.7478    -0.0473 0.400 0.220 0.000 0.336 0.044
#&gt; SRR1617446     1  0.0451     0.9007 0.988 0.000 0.000 0.004 0.008
#&gt; SRR1617447     1  0.0451     0.9007 0.988 0.000 0.000 0.004 0.008
#&gt; SRR1617448     1  0.0794     0.8986 0.972 0.000 0.000 0.028 0.000
#&gt; SRR1617449     1  0.0794     0.8986 0.972 0.000 0.000 0.028 0.000
#&gt; SRR1617451     2  0.0162     0.8529 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1617450     2  0.0162     0.8529 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1617452     4  0.1485     0.8799 0.000 0.032 0.000 0.948 0.020
#&gt; SRR1617454     2  0.0162     0.8523 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1617453     4  0.1485     0.8799 0.000 0.032 0.000 0.948 0.020
#&gt; SRR1617456     2  0.3932     0.7851 0.000 0.672 0.000 0.000 0.328
#&gt; SRR1617457     2  0.3932     0.7851 0.000 0.672 0.000 0.000 0.328
#&gt; SRR1617455     2  0.0162     0.8523 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1617458     2  0.3932     0.7851 0.000 0.672 0.000 0.000 0.328
#&gt; SRR1617459     2  0.3932     0.7851 0.000 0.672 0.000 0.000 0.328
#&gt; SRR1617460     5  0.5731     0.9948 0.000 0.104 0.000 0.328 0.568
#&gt; SRR1617461     5  0.5731     0.9948 0.000 0.104 0.000 0.328 0.568
#&gt; SRR1617463     5  0.5717     0.9949 0.000 0.104 0.000 0.324 0.572
#&gt; SRR1617462     5  0.5717     0.9949 0.000 0.104 0.000 0.324 0.572
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4 p5    p6
#&gt; SRR1617430     2  0.3652      0.499 0.000 0.672 0.000 0.000 NA 0.004
#&gt; SRR1617431     2  0.3652      0.499 0.000 0.672 0.000 0.000 NA 0.004
#&gt; SRR1617410     1  0.1138      0.828 0.960 0.000 0.000 0.004 NA 0.012
#&gt; SRR1617411     1  0.1138      0.828 0.960 0.000 0.000 0.004 NA 0.012
#&gt; SRR1617412     3  0.0000      0.905 0.000 0.000 1.000 0.000 NA 0.000
#&gt; SRR1617413     3  0.0000      0.905 0.000 0.000 1.000 0.000 NA 0.000
#&gt; SRR1617414     1  0.5655      0.589 0.548 0.000 0.000 0.180 NA 0.004
#&gt; SRR1617415     1  0.5655      0.589 0.548 0.000 0.000 0.180 NA 0.004
#&gt; SRR1617416     4  0.2979      0.861 0.004 0.000 0.000 0.804 NA 0.188
#&gt; SRR1617417     4  0.2979      0.861 0.004 0.000 0.000 0.804 NA 0.188
#&gt; SRR1617418     3  0.0000      0.905 0.000 0.000 1.000 0.000 NA 0.000
#&gt; SRR1617419     3  0.0000      0.905 0.000 0.000 1.000 0.000 NA 0.000
#&gt; SRR1617420     1  0.2554      0.828 0.876 0.000 0.000 0.028 NA 0.004
#&gt; SRR1617421     1  0.2554      0.828 0.876 0.000 0.000 0.028 NA 0.004
#&gt; SRR1617422     1  0.4535      0.748 0.704 0.000 0.000 0.148 NA 0.000
#&gt; SRR1617423     1  0.4535      0.748 0.704 0.000 0.000 0.148 NA 0.000
#&gt; SRR1617424     1  0.0146      0.836 0.996 0.000 0.000 0.000 NA 0.000
#&gt; SRR1617425     1  0.0405      0.837 0.988 0.000 0.000 0.004 NA 0.000
#&gt; SRR1617427     1  0.4377      0.756 0.720 0.000 0.000 0.160 NA 0.000
#&gt; SRR1617426     1  0.4377      0.756 0.720 0.000 0.000 0.160 NA 0.000
#&gt; SRR1617428     4  0.3934      0.712 0.008 0.000 0.000 0.616 NA 0.376
#&gt; SRR1617429     4  0.3934      0.712 0.008 0.000 0.000 0.616 NA 0.376
#&gt; SRR1617432     1  0.2030      0.803 0.908 0.000 0.000 0.000 NA 0.028
#&gt; SRR1617433     1  0.2030      0.803 0.908 0.000 0.000 0.000 NA 0.028
#&gt; SRR1617434     1  0.2030      0.803 0.908 0.000 0.000 0.000 NA 0.028
#&gt; SRR1617436     3  0.0458      0.897 0.000 0.000 0.984 0.000 NA 0.000
#&gt; SRR1617435     1  0.2173      0.803 0.904 0.000 0.000 0.004 NA 0.028
#&gt; SRR1617437     3  0.0458      0.897 0.000 0.000 0.984 0.000 NA 0.000
#&gt; SRR1617438     3  0.0000      0.905 0.000 0.000 1.000 0.000 NA 0.000
#&gt; SRR1617439     3  0.0000      0.905 0.000 0.000 1.000 0.000 NA 0.000
#&gt; SRR1617440     3  0.5571      0.317 0.000 0.144 0.484 0.000 NA 0.000
#&gt; SRR1617441     3  0.5571      0.317 0.000 0.144 0.484 0.000 NA 0.000
#&gt; SRR1617443     3  0.0000      0.905 0.000 0.000 1.000 0.000 NA 0.000
#&gt; SRR1617442     3  0.0000      0.905 0.000 0.000 1.000 0.000 NA 0.000
#&gt; SRR1617444     6  0.6604      0.297 0.296 0.008 0.000 0.012 NA 0.372
#&gt; SRR1617445     6  0.6604      0.297 0.296 0.008 0.000 0.012 NA 0.372
#&gt; SRR1617446     1  0.0000      0.836 1.000 0.000 0.000 0.000 NA 0.000
#&gt; SRR1617447     1  0.0000      0.836 1.000 0.000 0.000 0.000 NA 0.000
#&gt; SRR1617448     1  0.3550      0.802 0.816 0.000 0.000 0.044 NA 0.020
#&gt; SRR1617449     1  0.3550      0.802 0.816 0.000 0.000 0.044 NA 0.020
#&gt; SRR1617451     2  0.0146      0.717 0.000 0.996 0.000 0.000 NA 0.004
#&gt; SRR1617450     2  0.0146      0.717 0.000 0.996 0.000 0.000 NA 0.004
#&gt; SRR1617452     4  0.3345      0.859 0.000 0.028 0.000 0.788 NA 0.184
#&gt; SRR1617454     2  0.0146      0.717 0.000 0.996 0.000 0.004 NA 0.000
#&gt; SRR1617453     4  0.3345      0.859 0.000 0.028 0.000 0.788 NA 0.184
#&gt; SRR1617456     2  0.3847      0.655 0.000 0.544 0.000 0.000 NA 0.000
#&gt; SRR1617457     2  0.3847      0.655 0.000 0.544 0.000 0.000 NA 0.000
#&gt; SRR1617455     2  0.0146      0.717 0.000 0.996 0.000 0.004 NA 0.000
#&gt; SRR1617458     2  0.3847      0.655 0.000 0.544 0.000 0.000 NA 0.000
#&gt; SRR1617459     2  0.3847      0.655 0.000 0.544 0.000 0.000 NA 0.000
#&gt; SRR1617460     6  0.0713      0.603 0.000 0.028 0.000 0.000 NA 0.972
#&gt; SRR1617461     6  0.0713      0.603 0.000 0.028 0.000 0.000 NA 0.972
#&gt; SRR1617463     6  0.0713      0.603 0.000 0.028 0.000 0.000 NA 0.972
#&gt; SRR1617462     6  0.0713      0.603 0.000 0.028 0.000 0.000 NA 0.972
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.974       0.989         0.4691 0.525   0.525
#> 3 3 0.973           0.965       0.983         0.3972 0.793   0.616
#> 4 4 0.853           0.824       0.812         0.1117 0.866   0.627
#> 5 5 0.946           0.958       0.954         0.0833 0.964   0.850
#> 6 6 0.916           0.848       0.902         0.0468 0.944   0.743
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 5
```

There is also optional best $k$ = 2 3 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2   0.000      0.968 0.000 1.000
#&gt; SRR1617431     2   0.000      0.968 0.000 1.000
#&gt; SRR1617410     1   0.000      1.000 1.000 0.000
#&gt; SRR1617411     1   0.000      1.000 1.000 0.000
#&gt; SRR1617412     1   0.000      1.000 1.000 0.000
#&gt; SRR1617413     1   0.000      1.000 1.000 0.000
#&gt; SRR1617414     1   0.000      1.000 1.000 0.000
#&gt; SRR1617415     1   0.000      1.000 1.000 0.000
#&gt; SRR1617416     1   0.000      1.000 1.000 0.000
#&gt; SRR1617417     1   0.000      1.000 1.000 0.000
#&gt; SRR1617418     1   0.000      1.000 1.000 0.000
#&gt; SRR1617419     1   0.000      1.000 1.000 0.000
#&gt; SRR1617420     1   0.000      1.000 1.000 0.000
#&gt; SRR1617421     1   0.000      1.000 1.000 0.000
#&gt; SRR1617422     1   0.000      1.000 1.000 0.000
#&gt; SRR1617423     1   0.000      1.000 1.000 0.000
#&gt; SRR1617424     1   0.000      1.000 1.000 0.000
#&gt; SRR1617425     1   0.000      1.000 1.000 0.000
#&gt; SRR1617427     1   0.000      1.000 1.000 0.000
#&gt; SRR1617426     1   0.000      1.000 1.000 0.000
#&gt; SRR1617428     2   0.904      0.553 0.320 0.680
#&gt; SRR1617429     2   0.850      0.635 0.276 0.724
#&gt; SRR1617432     1   0.000      1.000 1.000 0.000
#&gt; SRR1617433     1   0.000      1.000 1.000 0.000
#&gt; SRR1617434     1   0.000      1.000 1.000 0.000
#&gt; SRR1617436     1   0.000      1.000 1.000 0.000
#&gt; SRR1617435     1   0.000      1.000 1.000 0.000
#&gt; SRR1617437     1   0.000      1.000 1.000 0.000
#&gt; SRR1617438     1   0.000      1.000 1.000 0.000
#&gt; SRR1617439     1   0.000      1.000 1.000 0.000
#&gt; SRR1617440     2   0.000      0.968 0.000 1.000
#&gt; SRR1617441     2   0.000      0.968 0.000 1.000
#&gt; SRR1617443     1   0.000      1.000 1.000 0.000
#&gt; SRR1617442     1   0.000      1.000 1.000 0.000
#&gt; SRR1617444     1   0.000      1.000 1.000 0.000
#&gt; SRR1617445     1   0.000      1.000 1.000 0.000
#&gt; SRR1617446     1   0.000      1.000 1.000 0.000
#&gt; SRR1617447     1   0.000      1.000 1.000 0.000
#&gt; SRR1617448     1   0.000      1.000 1.000 0.000
#&gt; SRR1617449     1   0.000      1.000 1.000 0.000
#&gt; SRR1617451     2   0.000      0.968 0.000 1.000
#&gt; SRR1617450     2   0.000      0.968 0.000 1.000
#&gt; SRR1617452     2   0.000      0.968 0.000 1.000
#&gt; SRR1617454     2   0.000      0.968 0.000 1.000
#&gt; SRR1617453     2   0.000      0.968 0.000 1.000
#&gt; SRR1617456     2   0.000      0.968 0.000 1.000
#&gt; SRR1617457     2   0.000      0.968 0.000 1.000
#&gt; SRR1617455     2   0.000      0.968 0.000 1.000
#&gt; SRR1617458     2   0.000      0.968 0.000 1.000
#&gt; SRR1617459     2   0.000      0.968 0.000 1.000
#&gt; SRR1617460     2   0.000      0.968 0.000 1.000
#&gt; SRR1617461     2   0.000      0.968 0.000 1.000
#&gt; SRR1617463     2   0.000      0.968 0.000 1.000
#&gt; SRR1617462     2   0.000      0.968 0.000 1.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617431     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617410     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617411     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617412     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617413     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617414     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617415     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617416     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617417     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617418     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617419     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617420     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617421     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617422     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617423     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617424     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617425     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617427     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617426     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617428     2   0.745      0.655 0.184 0.696 0.120
#&gt; SRR1617429     2   0.714      0.686 0.160 0.720 0.120
#&gt; SRR1617432     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617433     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617434     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617436     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617435     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617437     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617438     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617439     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617440     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617441     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617443     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617442     3   0.000      1.000 0.000 0.000 1.000
#&gt; SRR1617444     1   0.532      0.817 0.824 0.072 0.104
#&gt; SRR1617445     1   0.542      0.813 0.820 0.080 0.100
#&gt; SRR1617446     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617447     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617448     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617449     1   0.000      0.985 1.000 0.000 0.000
#&gt; SRR1617451     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617450     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617452     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617454     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617453     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617456     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617457     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617455     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617458     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617459     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617460     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617461     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617463     2   0.000      0.966 0.000 1.000 0.000
#&gt; SRR1617462     2   0.000      0.966 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.4977      0.564 0.000 0.540 0.000 0.460
#&gt; SRR1617431     2  0.4977      0.564 0.000 0.540 0.000 0.460
#&gt; SRR1617410     4  0.4999      0.709 0.492 0.000 0.000 0.508
#&gt; SRR1617411     4  0.4999      0.709 0.492 0.000 0.000 0.508
#&gt; SRR1617412     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1617413     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1617414     4  0.4998      0.706 0.488 0.000 0.000 0.512
#&gt; SRR1617415     4  0.4998      0.706 0.488 0.000 0.000 0.512
#&gt; SRR1617416     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; SRR1617417     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; SRR1617418     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1617419     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1617420     4  0.4999      0.709 0.492 0.000 0.000 0.508
#&gt; SRR1617421     4  0.4999      0.709 0.492 0.000 0.000 0.508
#&gt; SRR1617422     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0592      0.894 0.984 0.000 0.000 0.016
#&gt; SRR1617426     1  0.0592      0.894 0.984 0.000 0.000 0.016
#&gt; SRR1617428     4  0.7774     -0.339 0.060 0.324 0.084 0.532
#&gt; SRR1617429     4  0.7774     -0.339 0.060 0.324 0.084 0.532
#&gt; SRR1617432     4  0.4999      0.709 0.492 0.000 0.000 0.508
#&gt; SRR1617433     4  0.4999      0.709 0.492 0.000 0.000 0.508
#&gt; SRR1617434     4  0.4999      0.709 0.492 0.000 0.000 0.508
#&gt; SRR1617436     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1617435     4  0.4999      0.709 0.492 0.000 0.000 0.508
#&gt; SRR1617437     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1617438     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1617439     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1617440     3  0.0336      0.994 0.000 0.000 0.992 0.008
#&gt; SRR1617441     3  0.0336      0.994 0.000 0.000 0.992 0.008
#&gt; SRR1617443     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1617442     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1617444     1  0.4604      0.548 0.756 0.224 0.008 0.012
#&gt; SRR1617445     1  0.4533      0.540 0.752 0.232 0.004 0.012
#&gt; SRR1617446     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.915 1.000 0.000 0.000 0.000
#&gt; SRR1617451     2  0.2149      0.892 0.000 0.912 0.000 0.088
#&gt; SRR1617450     2  0.2216      0.890 0.000 0.908 0.000 0.092
#&gt; SRR1617452     2  0.0188      0.931 0.000 0.996 0.000 0.004
#&gt; SRR1617454     2  0.0817      0.925 0.000 0.976 0.000 0.024
#&gt; SRR1617453     2  0.0188      0.931 0.000 0.996 0.000 0.004
#&gt; SRR1617456     2  0.0336      0.931 0.000 0.992 0.000 0.008
#&gt; SRR1617457     2  0.0336      0.931 0.000 0.992 0.000 0.008
#&gt; SRR1617455     2  0.0817      0.925 0.000 0.976 0.000 0.024
#&gt; SRR1617458     2  0.0336      0.931 0.000 0.992 0.000 0.008
#&gt; SRR1617459     2  0.0336      0.931 0.000 0.992 0.000 0.008
#&gt; SRR1617460     2  0.0000      0.932 0.000 1.000 0.000 0.000
#&gt; SRR1617461     2  0.0000      0.932 0.000 1.000 0.000 0.000
#&gt; SRR1617463     2  0.0000      0.932 0.000 1.000 0.000 0.000
#&gt; SRR1617462     2  0.0000      0.932 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     4  0.0794      0.975 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1617431     4  0.0794      0.975 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1617410     5  0.1732      0.998 0.080 0.000 0.000 0.000 0.920
#&gt; SRR1617411     5  0.1732      0.998 0.080 0.000 0.000 0.000 0.920
#&gt; SRR1617412     3  0.0912      0.970 0.000 0.000 0.972 0.016 0.012
#&gt; SRR1617413     3  0.0912      0.970 0.000 0.000 0.972 0.016 0.012
#&gt; SRR1617414     5  0.1732      0.998 0.080 0.000 0.000 0.000 0.920
#&gt; SRR1617415     5  0.1732      0.998 0.080 0.000 0.000 0.000 0.920
#&gt; SRR1617416     1  0.0609      0.973 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1617417     1  0.0609      0.973 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1617418     3  0.0798      0.972 0.000 0.000 0.976 0.016 0.008
#&gt; SRR1617419     3  0.0798      0.972 0.000 0.000 0.976 0.016 0.008
#&gt; SRR1617420     5  0.1732      0.998 0.080 0.000 0.000 0.000 0.920
#&gt; SRR1617421     5  0.1732      0.998 0.080 0.000 0.000 0.000 0.920
#&gt; SRR1617422     1  0.0290      0.976 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1617423     1  0.0290      0.976 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1617424     1  0.0404      0.976 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1617425     1  0.0404      0.976 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1617427     1  0.0865      0.968 0.972 0.000 0.000 0.004 0.024
#&gt; SRR1617426     1  0.0865      0.968 0.972 0.000 0.000 0.004 0.024
#&gt; SRR1617428     4  0.0960      0.975 0.004 0.000 0.008 0.972 0.016
#&gt; SRR1617429     4  0.0960      0.975 0.004 0.000 0.008 0.972 0.016
#&gt; SRR1617432     5  0.1892      0.997 0.080 0.000 0.000 0.004 0.916
#&gt; SRR1617433     5  0.1892      0.997 0.080 0.000 0.000 0.004 0.916
#&gt; SRR1617434     5  0.1892      0.997 0.080 0.000 0.000 0.004 0.916
#&gt; SRR1617436     3  0.0000      0.977 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617435     5  0.1892      0.997 0.080 0.000 0.000 0.004 0.916
#&gt; SRR1617437     3  0.0000      0.977 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617438     3  0.0000      0.977 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617439     3  0.0000      0.977 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617440     3  0.2130      0.921 0.000 0.016 0.924 0.016 0.044
#&gt; SRR1617441     3  0.2130      0.921 0.000 0.016 0.924 0.016 0.044
#&gt; SRR1617443     3  0.0000      0.977 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617442     3  0.0000      0.977 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617444     1  0.2288      0.906 0.924 0.020 0.020 0.008 0.028
#&gt; SRR1617445     1  0.2288      0.906 0.924 0.020 0.020 0.008 0.028
#&gt; SRR1617446     1  0.0290      0.977 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1617447     1  0.0290      0.977 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1617448     1  0.0000      0.974 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.974 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617451     2  0.3438      0.824 0.000 0.808 0.000 0.172 0.020
#&gt; SRR1617450     2  0.3438      0.824 0.000 0.808 0.000 0.172 0.020
#&gt; SRR1617452     2  0.0968      0.932 0.004 0.972 0.000 0.012 0.012
#&gt; SRR1617454     2  0.1942      0.911 0.000 0.920 0.000 0.068 0.012
#&gt; SRR1617453     2  0.0968      0.932 0.004 0.972 0.000 0.012 0.012
#&gt; SRR1617456     2  0.1626      0.928 0.000 0.940 0.000 0.016 0.044
#&gt; SRR1617457     2  0.1626      0.928 0.000 0.940 0.000 0.016 0.044
#&gt; SRR1617455     2  0.1877      0.913 0.000 0.924 0.000 0.064 0.012
#&gt; SRR1617458     2  0.1626      0.928 0.000 0.940 0.000 0.016 0.044
#&gt; SRR1617459     2  0.1626      0.928 0.000 0.940 0.000 0.016 0.044
#&gt; SRR1617460     2  0.1211      0.925 0.024 0.960 0.000 0.000 0.016
#&gt; SRR1617461     2  0.1211      0.925 0.024 0.960 0.000 0.000 0.016
#&gt; SRR1617463     2  0.1117      0.927 0.020 0.964 0.000 0.000 0.016
#&gt; SRR1617462     2  0.1117      0.927 0.020 0.964 0.000 0.000 0.016
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     4  0.0363     0.9913 0.000 0.012 0.000 0.988 0.000 0.000
#&gt; SRR1617431     4  0.0363     0.9913 0.000 0.012 0.000 0.988 0.000 0.000
#&gt; SRR1617410     5  0.0000     0.9985 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617411     5  0.0000     0.9985 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617412     3  0.0790     0.8613 0.000 0.000 0.968 0.000 0.000 0.032
#&gt; SRR1617413     3  0.0790     0.8613 0.000 0.000 0.968 0.000 0.000 0.032
#&gt; SRR1617414     5  0.0146     0.9978 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1617415     5  0.0146     0.9978 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1617416     1  0.1074     0.9611 0.960 0.012 0.000 0.000 0.000 0.028
#&gt; SRR1617417     1  0.1074     0.9611 0.960 0.012 0.000 0.000 0.000 0.028
#&gt; SRR1617418     3  0.0000     0.8778 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617419     3  0.0000     0.8778 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617420     5  0.0000     0.9985 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617421     5  0.0000     0.9985 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617422     1  0.0508     0.9757 0.984 0.004 0.000 0.000 0.000 0.012
#&gt; SRR1617423     1  0.0508     0.9757 0.984 0.004 0.000 0.000 0.000 0.012
#&gt; SRR1617424     1  0.0000     0.9805 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000     0.9805 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0146     0.9794 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0146     0.9794 0.996 0.000 0.004 0.000 0.000 0.000
#&gt; SRR1617428     4  0.0000     0.9913 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617429     4  0.0000     0.9913 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617432     5  0.0146     0.9978 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1617433     5  0.0146     0.9978 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1617434     5  0.0000     0.9985 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617436     3  0.2778     0.9120 0.000 0.000 0.824 0.008 0.000 0.168
#&gt; SRR1617435     5  0.0000     0.9985 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617437     3  0.2778     0.9120 0.000 0.000 0.824 0.008 0.000 0.168
#&gt; SRR1617438     3  0.2793     0.8995 0.000 0.000 0.800 0.000 0.000 0.200
#&gt; SRR1617439     3  0.2793     0.8995 0.000 0.000 0.800 0.000 0.000 0.200
#&gt; SRR1617440     6  0.2378     0.4912 0.000 0.000 0.152 0.000 0.000 0.848
#&gt; SRR1617441     6  0.2378     0.4912 0.000 0.000 0.152 0.000 0.000 0.848
#&gt; SRR1617443     3  0.2527     0.9126 0.000 0.000 0.832 0.000 0.000 0.168
#&gt; SRR1617442     3  0.2527     0.9126 0.000 0.000 0.832 0.000 0.000 0.168
#&gt; SRR1617444     1  0.1686     0.9345 0.924 0.012 0.000 0.000 0.000 0.064
#&gt; SRR1617445     1  0.1686     0.9345 0.924 0.012 0.000 0.000 0.000 0.064
#&gt; SRR1617446     1  0.0000     0.9805 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000     0.9805 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0000     0.9805 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000     0.9805 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617451     6  0.5969     0.4451 0.000 0.244 0.000 0.316 0.000 0.440
#&gt; SRR1617450     6  0.5969     0.4451 0.000 0.244 0.000 0.316 0.000 0.440
#&gt; SRR1617452     2  0.3804     0.0699 0.000 0.576 0.000 0.000 0.000 0.424
#&gt; SRR1617454     2  0.1124     0.7900 0.000 0.956 0.000 0.036 0.000 0.008
#&gt; SRR1617453     2  0.3804     0.0699 0.000 0.576 0.000 0.000 0.000 0.424
#&gt; SRR1617456     6  0.3309     0.6366 0.000 0.280 0.000 0.000 0.000 0.720
#&gt; SRR1617457     6  0.3309     0.6366 0.000 0.280 0.000 0.000 0.000 0.720
#&gt; SRR1617455     2  0.1124     0.7900 0.000 0.956 0.000 0.036 0.000 0.008
#&gt; SRR1617458     6  0.3309     0.6366 0.000 0.280 0.000 0.000 0.000 0.720
#&gt; SRR1617459     6  0.3309     0.6366 0.000 0.280 0.000 0.000 0.000 0.720
#&gt; SRR1617460     2  0.0508     0.8022 0.004 0.984 0.000 0.000 0.000 0.012
#&gt; SRR1617461     2  0.0508     0.8022 0.004 0.984 0.000 0.000 0.000 0.012
#&gt; SRR1617463     2  0.0146     0.8027 0.004 0.996 0.000 0.000 0.000 0.000
#&gt; SRR1617462     2  0.0000     0.8038 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.0736 0.927   0.927
#> 3 3 1.000           0.972       0.988         2.0760 0.866   0.855
#> 4 4 0.665           0.661       0.886         0.7175 0.877   0.845
#> 5 5 0.716           0.872       0.928         0.2971 0.776   0.672
#> 6 6 0.925           0.865       0.927         0.0683 0.997   0.994
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1617430     2       0          1  0  1
#&gt; SRR1617431     2       0          1  0  1
#&gt; SRR1617410     1       0          1  1  0
#&gt; SRR1617411     1       0          1  1  0
#&gt; SRR1617412     1       0          1  1  0
#&gt; SRR1617413     1       0          1  1  0
#&gt; SRR1617414     1       0          1  1  0
#&gt; SRR1617415     1       0          1  1  0
#&gt; SRR1617416     1       0          1  1  0
#&gt; SRR1617417     1       0          1  1  0
#&gt; SRR1617418     1       0          1  1  0
#&gt; SRR1617419     1       0          1  1  0
#&gt; SRR1617420     1       0          1  1  0
#&gt; SRR1617421     1       0          1  1  0
#&gt; SRR1617422     1       0          1  1  0
#&gt; SRR1617423     1       0          1  1  0
#&gt; SRR1617424     1       0          1  1  0
#&gt; SRR1617425     1       0          1  1  0
#&gt; SRR1617427     1       0          1  1  0
#&gt; SRR1617426     1       0          1  1  0
#&gt; SRR1617428     1       0          1  1  0
#&gt; SRR1617429     1       0          1  1  0
#&gt; SRR1617432     1       0          1  1  0
#&gt; SRR1617433     1       0          1  1  0
#&gt; SRR1617434     1       0          1  1  0
#&gt; SRR1617436     1       0          1  1  0
#&gt; SRR1617435     1       0          1  1  0
#&gt; SRR1617437     1       0          1  1  0
#&gt; SRR1617438     1       0          1  1  0
#&gt; SRR1617439     1       0          1  1  0
#&gt; SRR1617440     1       0          1  1  0
#&gt; SRR1617441     1       0          1  1  0
#&gt; SRR1617443     1       0          1  1  0
#&gt; SRR1617442     1       0          1  1  0
#&gt; SRR1617444     1       0          1  1  0
#&gt; SRR1617445     1       0          1  1  0
#&gt; SRR1617446     1       0          1  1  0
#&gt; SRR1617447     1       0          1  1  0
#&gt; SRR1617448     1       0          1  1  0
#&gt; SRR1617449     1       0          1  1  0
#&gt; SRR1617451     1       0          1  1  0
#&gt; SRR1617450     1       0          1  1  0
#&gt; SRR1617452     1       0          1  1  0
#&gt; SRR1617454     1       0          1  1  0
#&gt; SRR1617453     1       0          1  1  0
#&gt; SRR1617456     1       0          1  1  0
#&gt; SRR1617457     1       0          1  1  0
#&gt; SRR1617455     1       0          1  1  0
#&gt; SRR1617458     1       0          1  1  0
#&gt; SRR1617459     1       0          1  1  0
#&gt; SRR1617460     1       0          1  1  0
#&gt; SRR1617461     1       0          1  1  0
#&gt; SRR1617463     1       0          1  1  0
#&gt; SRR1617462     1       0          1  1  0
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3
#&gt; SRR1617430     3  0.0000      1.000 0.000 0.000  1
#&gt; SRR1617431     3  0.0000      1.000 0.000 0.000  1
#&gt; SRR1617410     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617411     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617412     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617413     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617414     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617415     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617416     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617417     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617418     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617419     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617420     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617421     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617422     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617423     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617424     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617425     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617427     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617426     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617428     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617429     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617432     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617433     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617434     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617436     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617435     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617437     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617438     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617439     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617440     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617441     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617443     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617442     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617444     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617445     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617446     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617447     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617448     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617449     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617451     2  0.0000      1.000 0.000 1.000  0
#&gt; SRR1617450     2  0.0000      1.000 0.000 1.000  0
#&gt; SRR1617452     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617454     2  0.0000      1.000 0.000 1.000  0
#&gt; SRR1617453     1  0.0000      0.986 1.000 0.000  0
#&gt; SRR1617456     1  0.0424      0.980 0.992 0.008  0
#&gt; SRR1617457     1  0.0424      0.980 0.992 0.008  0
#&gt; SRR1617455     2  0.0000      1.000 0.000 1.000  0
#&gt; SRR1617458     1  0.0424      0.980 0.992 0.008  0
#&gt; SRR1617459     1  0.0424      0.980 0.992 0.008  0
#&gt; SRR1617460     1  0.0592      0.977 0.988 0.012  0
#&gt; SRR1617461     1  0.0592      0.977 0.988 0.012  0
#&gt; SRR1617463     1  0.5560      0.585 0.700 0.300  0
#&gt; SRR1617462     1  0.5560      0.585 0.700 0.300  0
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     3  0.4989     1.0000 0.000 0.000 0.528 0.472
#&gt; SRR1617431     3  0.4989     1.0000 0.000 0.000 0.528 0.472
#&gt; SRR1617410     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617411     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617412     1  0.0188     0.8008 0.996 0.000 0.004 0.000
#&gt; SRR1617413     1  0.0188     0.8008 0.996 0.000 0.004 0.000
#&gt; SRR1617414     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617415     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617416     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617417     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617418     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617419     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617420     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617421     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617422     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617428     1  0.4989     0.0616 0.528 0.000 0.472 0.000
#&gt; SRR1617429     1  0.4989     0.0616 0.528 0.000 0.472 0.000
#&gt; SRR1617432     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617433     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617434     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617436     1  0.4989     0.0616 0.528 0.000 0.472 0.000
#&gt; SRR1617435     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617437     1  0.4989     0.0616 0.528 0.000 0.472 0.000
#&gt; SRR1617438     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617439     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617440     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617441     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617443     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617442     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617444     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617445     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617446     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000     0.8059 1.000 0.000 0.000 0.000
#&gt; SRR1617451     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; SRR1617450     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; SRR1617452     1  0.3873     0.1501 0.772 0.000 0.000 0.228
#&gt; SRR1617454     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; SRR1617453     1  0.3873     0.1501 0.772 0.000 0.000 0.228
#&gt; SRR1617456     4  0.4992     1.0000 0.476 0.000 0.000 0.524
#&gt; SRR1617457     4  0.4992     1.0000 0.476 0.000 0.000 0.524
#&gt; SRR1617455     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; SRR1617458     4  0.4992     1.0000 0.476 0.000 0.000 0.524
#&gt; SRR1617459     4  0.4992     1.0000 0.476 0.000 0.000 0.524
#&gt; SRR1617460     1  0.4605    -0.4183 0.664 0.000 0.000 0.336
#&gt; SRR1617461     1  0.4605    -0.4183 0.664 0.000 0.000 0.336
#&gt; SRR1617463     1  0.7883    -0.6966 0.376 0.288 0.000 0.336
#&gt; SRR1617462     1  0.7883    -0.6966 0.376 0.288 0.000 0.336
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3 p4    p5
#&gt; SRR1617430     4  0.0000      1.000 0.000 0.000 0.000  1 0.000
#&gt; SRR1617431     4  0.0000      1.000 0.000 0.000 0.000  1 0.000
#&gt; SRR1617410     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617411     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617412     1  0.0162      0.959 0.996 0.000 0.004  0 0.000
#&gt; SRR1617413     1  0.0162      0.959 0.996 0.000 0.004  0 0.000
#&gt; SRR1617414     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617415     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617416     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617417     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617418     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617419     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617420     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617421     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617422     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617423     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617424     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617425     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617427     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617426     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617428     3  0.3366      0.998 0.212 0.004 0.784  0 0.000
#&gt; SRR1617429     3  0.3366      0.998 0.212 0.004 0.784  0 0.000
#&gt; SRR1617432     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617433     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617434     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617436     3  0.3210      0.998 0.212 0.000 0.788  0 0.000
#&gt; SRR1617435     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617437     3  0.3210      0.998 0.212 0.000 0.788  0 0.000
#&gt; SRR1617438     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617439     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617440     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617441     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617443     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617442     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617444     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617445     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617446     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617447     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617448     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617449     1  0.0000      0.964 1.000 0.000 0.000  0 0.000
#&gt; SRR1617451     5  0.0000      0.782 0.000 0.000 0.000  0 1.000
#&gt; SRR1617450     5  0.0000      0.782 0.000 0.000 0.000  0 1.000
#&gt; SRR1617452     1  0.4283     -0.087 0.544 0.456 0.000  0 0.000
#&gt; SRR1617454     5  0.4974      0.786 0.000 0.092 0.212  0 0.696
#&gt; SRR1617453     1  0.4283     -0.087 0.544 0.456 0.000  0 0.000
#&gt; SRR1617456     2  0.2074      0.782 0.104 0.896 0.000  0 0.000
#&gt; SRR1617457     2  0.2074      0.782 0.104 0.896 0.000  0 0.000
#&gt; SRR1617455     5  0.4974      0.786 0.000 0.092 0.212  0 0.696
#&gt; SRR1617458     2  0.2074      0.782 0.104 0.896 0.000  0 0.000
#&gt; SRR1617459     2  0.2074      0.782 0.104 0.896 0.000  0 0.000
#&gt; SRR1617460     2  0.3752      0.552 0.292 0.708 0.000  0 0.000
#&gt; SRR1617461     2  0.3752      0.552 0.292 0.708 0.000  0 0.000
#&gt; SRR1617463     2  0.3352      0.572 0.004 0.800 0.192  0 0.004
#&gt; SRR1617462     2  0.3352      0.572 0.004 0.800 0.192  0 0.004
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4 p5    p6
#&gt; SRR1617430     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; SRR1617431     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; SRR1617410     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617411     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617412     1  0.0363      0.957 0.988 0.000 0.000 0.012  0 0.000
#&gt; SRR1617413     1  0.0363      0.957 0.988 0.000 0.000 0.012  0 0.000
#&gt; SRR1617414     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617415     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617416     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617417     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617418     1  0.0260      0.960 0.992 0.000 0.000 0.008  0 0.000
#&gt; SRR1617419     1  0.0260      0.960 0.992 0.000 0.000 0.008  0 0.000
#&gt; SRR1617420     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617421     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617422     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617423     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617424     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617425     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617427     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617426     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617428     4  0.0865      1.000 0.036 0.000 0.000 0.964  0 0.000
#&gt; SRR1617429     4  0.0865      1.000 0.036 0.000 0.000 0.964  0 0.000
#&gt; SRR1617432     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617433     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617434     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617436     3  0.4453      1.000 0.036 0.000 0.592 0.372  0 0.000
#&gt; SRR1617435     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617437     3  0.4453      1.000 0.036 0.000 0.592 0.372  0 0.000
#&gt; SRR1617438     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617439     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617440     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617441     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617443     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617442     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617444     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617445     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617446     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617447     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617448     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617449     1  0.0000      0.967 1.000 0.000 0.000 0.000  0 0.000
#&gt; SRR1617451     2  0.0000      0.722 0.000 1.000 0.000 0.000  0 0.000
#&gt; SRR1617450     2  0.0000      0.722 0.000 1.000 0.000 0.000  0 0.000
#&gt; SRR1617452     1  0.4591     -0.128 0.500 0.000 0.000 0.036  0 0.464
#&gt; SRR1617454     2  0.5190      0.727 0.000 0.596 0.132 0.000  0 0.272
#&gt; SRR1617453     1  0.4591     -0.128 0.500 0.000 0.000 0.036  0 0.464
#&gt; SRR1617456     6  0.3534      0.744 0.008 0.000 0.276 0.000  0 0.716
#&gt; SRR1617457     6  0.3534      0.744 0.008 0.000 0.276 0.000  0 0.716
#&gt; SRR1617455     2  0.5190      0.727 0.000 0.596 0.132 0.000  0 0.272
#&gt; SRR1617458     6  0.3534      0.744 0.008 0.000 0.276 0.000  0 0.716
#&gt; SRR1617459     6  0.3534      0.744 0.008 0.000 0.276 0.000  0 0.716
#&gt; SRR1617460     6  0.3309      0.517 0.280 0.000 0.000 0.000  0 0.720
#&gt; SRR1617461     6  0.3309      0.517 0.280 0.000 0.000 0.000  0 0.720
#&gt; SRR1617463     6  0.0458      0.619 0.000 0.000 0.016 0.000  0 0.984
#&gt; SRR1617462     6  0.0458      0.619 0.000 0.000 0.016 0.000  0 0.984
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.620           0.849       0.912         0.2706 0.743   0.743
#> 3 3 0.229           0.669       0.759         0.7107 0.860   0.817
#> 4 4 0.275           0.470       0.653         0.3073 0.681   0.507
#> 5 5 0.318           0.372       0.630         0.1283 0.762   0.427
#> 6 6 0.385           0.580       0.662         0.0869 0.916   0.702
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.3733      0.783 0.072 0.928
#&gt; SRR1617431     2  0.3733      0.783 0.072 0.928
#&gt; SRR1617410     1  0.2236      0.914 0.964 0.036
#&gt; SRR1617411     1  0.2236      0.914 0.964 0.036
#&gt; SRR1617412     1  0.3114      0.906 0.944 0.056
#&gt; SRR1617413     1  0.3114      0.906 0.944 0.056
#&gt; SRR1617414     1  0.2423      0.913 0.960 0.040
#&gt; SRR1617415     1  0.2423      0.913 0.960 0.040
#&gt; SRR1617416     1  0.1414      0.919 0.980 0.020
#&gt; SRR1617417     1  0.1414      0.919 0.980 0.020
#&gt; SRR1617418     1  0.3274      0.906 0.940 0.060
#&gt; SRR1617419     1  0.3274      0.906 0.940 0.060
#&gt; SRR1617420     1  0.2423      0.913 0.960 0.040
#&gt; SRR1617421     1  0.2423      0.913 0.960 0.040
#&gt; SRR1617422     1  0.0938      0.919 0.988 0.012
#&gt; SRR1617423     1  0.0938      0.919 0.988 0.012
#&gt; SRR1617424     1  0.1184      0.919 0.984 0.016
#&gt; SRR1617425     1  0.1184      0.919 0.984 0.016
#&gt; SRR1617427     1  0.1633      0.918 0.976 0.024
#&gt; SRR1617426     1  0.1633      0.918 0.976 0.024
#&gt; SRR1617428     1  0.4815      0.837 0.896 0.104
#&gt; SRR1617429     1  0.4815      0.837 0.896 0.104
#&gt; SRR1617432     1  0.2423      0.913 0.960 0.040
#&gt; SRR1617433     1  0.2423      0.913 0.960 0.040
#&gt; SRR1617434     1  0.2236      0.914 0.964 0.036
#&gt; SRR1617436     1  0.2043      0.918 0.968 0.032
#&gt; SRR1617435     1  0.2236      0.914 0.964 0.036
#&gt; SRR1617437     1  0.2043      0.918 0.968 0.032
#&gt; SRR1617438     1  0.3114      0.904 0.944 0.056
#&gt; SRR1617439     1  0.3114      0.904 0.944 0.056
#&gt; SRR1617440     1  0.3114      0.904 0.944 0.056
#&gt; SRR1617441     1  0.3114      0.904 0.944 0.056
#&gt; SRR1617443     1  0.3114      0.904 0.944 0.056
#&gt; SRR1617442     1  0.3114      0.904 0.944 0.056
#&gt; SRR1617444     1  0.1414      0.918 0.980 0.020
#&gt; SRR1617445     1  0.1414      0.918 0.980 0.020
#&gt; SRR1617446     1  0.1184      0.919 0.984 0.016
#&gt; SRR1617447     1  0.1184      0.919 0.984 0.016
#&gt; SRR1617448     1  0.0938      0.919 0.988 0.012
#&gt; SRR1617449     1  0.0938      0.919 0.988 0.012
#&gt; SRR1617451     2  0.7815      0.873 0.232 0.768
#&gt; SRR1617450     2  0.7815      0.873 0.232 0.768
#&gt; SRR1617452     1  0.1843      0.916 0.972 0.028
#&gt; SRR1617454     2  0.8016      0.871 0.244 0.756
#&gt; SRR1617453     1  0.1843      0.916 0.972 0.028
#&gt; SRR1617456     1  0.9358      0.275 0.648 0.352
#&gt; SRR1617457     1  0.9358      0.275 0.648 0.352
#&gt; SRR1617455     2  0.8016      0.871 0.244 0.756
#&gt; SRR1617458     1  0.9358      0.275 0.648 0.352
#&gt; SRR1617459     1  0.9358      0.275 0.648 0.352
#&gt; SRR1617460     1  0.0938      0.919 0.988 0.012
#&gt; SRR1617461     1  0.0938      0.919 0.988 0.012
#&gt; SRR1617463     2  0.9460      0.730 0.364 0.636
#&gt; SRR1617462     2  0.9460      0.730 0.364 0.636
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.6229      0.572 0.008 0.652 0.340
#&gt; SRR1617431     2  0.6252      0.572 0.008 0.648 0.344
#&gt; SRR1617410     1  0.5171      0.689 0.784 0.012 0.204
#&gt; SRR1617411     1  0.5171      0.689 0.784 0.012 0.204
#&gt; SRR1617412     1  0.6016      0.700 0.724 0.020 0.256
#&gt; SRR1617413     1  0.6016      0.700 0.724 0.020 0.256
#&gt; SRR1617414     1  0.5360      0.679 0.768 0.012 0.220
#&gt; SRR1617415     1  0.5360      0.679 0.768 0.012 0.220
#&gt; SRR1617416     1  0.2590      0.766 0.924 0.004 0.072
#&gt; SRR1617417     1  0.2590      0.766 0.924 0.004 0.072
#&gt; SRR1617418     1  0.6597      0.675 0.696 0.036 0.268
#&gt; SRR1617419     1  0.6597      0.675 0.696 0.036 0.268
#&gt; SRR1617420     1  0.5360      0.687 0.768 0.012 0.220
#&gt; SRR1617421     1  0.5360      0.687 0.768 0.012 0.220
#&gt; SRR1617422     1  0.1337      0.765 0.972 0.012 0.016
#&gt; SRR1617423     1  0.1337      0.765 0.972 0.012 0.016
#&gt; SRR1617424     1  0.1015      0.765 0.980 0.012 0.008
#&gt; SRR1617425     1  0.1015      0.765 0.980 0.012 0.008
#&gt; SRR1617427     1  0.2804      0.756 0.924 0.016 0.060
#&gt; SRR1617426     1  0.2804      0.756 0.924 0.016 0.060
#&gt; SRR1617428     1  0.7205      0.611 0.708 0.100 0.192
#&gt; SRR1617429     1  0.7205      0.611 0.708 0.100 0.192
#&gt; SRR1617432     1  0.5360      0.679 0.768 0.012 0.220
#&gt; SRR1617433     1  0.5360      0.679 0.768 0.012 0.220
#&gt; SRR1617434     1  0.5171      0.690 0.784 0.012 0.204
#&gt; SRR1617436     1  0.6742      0.629 0.656 0.028 0.316
#&gt; SRR1617435     1  0.5171      0.690 0.784 0.012 0.204
#&gt; SRR1617437     1  0.6742      0.629 0.656 0.028 0.316
#&gt; SRR1617438     1  0.5817      0.701 0.744 0.020 0.236
#&gt; SRR1617439     1  0.5817      0.701 0.744 0.020 0.236
#&gt; SRR1617440     1  0.6646      0.673 0.712 0.048 0.240
#&gt; SRR1617441     1  0.6646      0.673 0.712 0.048 0.240
#&gt; SRR1617443     1  0.5858      0.705 0.740 0.020 0.240
#&gt; SRR1617442     1  0.5858      0.705 0.740 0.020 0.240
#&gt; SRR1617444     1  0.2550      0.758 0.932 0.012 0.056
#&gt; SRR1617445     1  0.2550      0.758 0.932 0.012 0.056
#&gt; SRR1617446     1  0.1015      0.765 0.980 0.012 0.008
#&gt; SRR1617447     1  0.1015      0.765 0.980 0.012 0.008
#&gt; SRR1617448     1  0.0661      0.766 0.988 0.004 0.008
#&gt; SRR1617449     1  0.0661      0.766 0.988 0.004 0.008
#&gt; SRR1617451     2  0.5094      0.713 0.112 0.832 0.056
#&gt; SRR1617450     2  0.5094      0.713 0.112 0.832 0.056
#&gt; SRR1617452     1  0.7807      0.567 0.656 0.108 0.236
#&gt; SRR1617454     2  0.3500      0.715 0.116 0.880 0.004
#&gt; SRR1617453     1  0.7807      0.567 0.656 0.108 0.236
#&gt; SRR1617456     2  0.9649      0.416 0.388 0.404 0.208
#&gt; SRR1617457     2  0.9649      0.416 0.388 0.404 0.208
#&gt; SRR1617455     2  0.3500      0.715 0.116 0.880 0.004
#&gt; SRR1617458     2  0.9649      0.416 0.388 0.404 0.208
#&gt; SRR1617459     2  0.9649      0.416 0.388 0.404 0.208
#&gt; SRR1617460     1  0.7451      0.523 0.700 0.156 0.144
#&gt; SRR1617461     1  0.7451      0.523 0.700 0.156 0.144
#&gt; SRR1617463     2  0.6586      0.682 0.216 0.728 0.056
#&gt; SRR1617462     2  0.6586      0.682 0.216 0.728 0.056
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     4   0.517    0.99526 0.004 0.484 0.000 0.512
#&gt; SRR1617431     4   0.530    0.99526 0.008 0.488 0.000 0.504
#&gt; SRR1617410     1   0.381    0.64028 0.812 0.000 0.176 0.012
#&gt; SRR1617411     1   0.381    0.64028 0.812 0.000 0.176 0.012
#&gt; SRR1617412     3   0.344    0.57646 0.084 0.000 0.868 0.048
#&gt; SRR1617413     3   0.344    0.57646 0.084 0.000 0.868 0.048
#&gt; SRR1617414     1   0.371    0.62660 0.836 0.000 0.140 0.024
#&gt; SRR1617415     1   0.371    0.62660 0.836 0.000 0.140 0.024
#&gt; SRR1617416     3   0.752   -0.06201 0.380 0.036 0.500 0.084
#&gt; SRR1617417     3   0.752   -0.06201 0.380 0.036 0.500 0.084
#&gt; SRR1617418     3   0.260    0.58658 0.040 0.004 0.916 0.040
#&gt; SRR1617419     3   0.260    0.58658 0.040 0.004 0.916 0.040
#&gt; SRR1617420     1   0.428    0.62836 0.780 0.000 0.200 0.020
#&gt; SRR1617421     1   0.428    0.62836 0.780 0.000 0.200 0.020
#&gt; SRR1617422     1   0.712    0.44434 0.500 0.040 0.412 0.048
#&gt; SRR1617423     1   0.712    0.44434 0.500 0.040 0.412 0.048
#&gt; SRR1617424     1   0.719    0.44690 0.496 0.044 0.412 0.048
#&gt; SRR1617425     1   0.719    0.44690 0.496 0.044 0.412 0.048
#&gt; SRR1617427     1   0.707    0.49403 0.544 0.036 0.364 0.056
#&gt; SRR1617426     1   0.707    0.49403 0.544 0.036 0.364 0.056
#&gt; SRR1617428     3   0.842    0.25494 0.256 0.084 0.524 0.136
#&gt; SRR1617429     3   0.842    0.25494 0.256 0.084 0.524 0.136
#&gt; SRR1617432     1   0.366    0.62502 0.840 0.000 0.136 0.024
#&gt; SRR1617433     1   0.366    0.62502 0.840 0.000 0.136 0.024
#&gt; SRR1617434     1   0.374    0.63923 0.824 0.000 0.160 0.016
#&gt; SRR1617436     3   0.550    0.50676 0.112 0.004 0.744 0.140
#&gt; SRR1617435     1   0.374    0.63923 0.824 0.000 0.160 0.016
#&gt; SRR1617437     3   0.550    0.50676 0.112 0.004 0.744 0.140
#&gt; SRR1617438     3   0.252    0.59656 0.076 0.000 0.908 0.016
#&gt; SRR1617439     3   0.252    0.59656 0.076 0.000 0.908 0.016
#&gt; SRR1617440     3   0.310    0.59274 0.080 0.004 0.888 0.028
#&gt; SRR1617441     3   0.310    0.59274 0.080 0.004 0.888 0.028
#&gt; SRR1617443     3   0.317    0.58634 0.116 0.000 0.868 0.016
#&gt; SRR1617442     3   0.317    0.58634 0.116 0.000 0.868 0.016
#&gt; SRR1617444     3   0.724    0.00535 0.348 0.048 0.548 0.056
#&gt; SRR1617445     3   0.724    0.00535 0.348 0.048 0.548 0.056
#&gt; SRR1617446     1   0.734    0.39505 0.468 0.048 0.432 0.052
#&gt; SRR1617447     1   0.734    0.39505 0.468 0.048 0.432 0.052
#&gt; SRR1617448     1   0.728    0.34695 0.452 0.048 0.452 0.048
#&gt; SRR1617449     3   0.728   -0.40822 0.452 0.048 0.452 0.048
#&gt; SRR1617451     2   0.490    0.32785 0.060 0.816 0.060 0.064
#&gt; SRR1617450     2   0.490    0.32785 0.060 0.816 0.060 0.064
#&gt; SRR1617452     3   0.816    0.44861 0.216 0.080 0.564 0.140
#&gt; SRR1617454     2   0.267    0.43281 0.044 0.908 0.048 0.000
#&gt; SRR1617453     3   0.816    0.44861 0.216 0.080 0.564 0.140
#&gt; SRR1617456     2   0.939    0.61849 0.148 0.412 0.272 0.168
#&gt; SRR1617457     2   0.939    0.61849 0.148 0.412 0.272 0.168
#&gt; SRR1617455     2   0.267    0.43281 0.044 0.908 0.048 0.000
#&gt; SRR1617458     2   0.941    0.60845 0.144 0.404 0.280 0.172
#&gt; SRR1617459     2   0.941    0.60845 0.144 0.404 0.280 0.172
#&gt; SRR1617460     3   0.966    0.10144 0.260 0.232 0.360 0.148
#&gt; SRR1617461     3   0.966    0.10144 0.260 0.232 0.360 0.148
#&gt; SRR1617463     2   0.673    0.59660 0.096 0.704 0.096 0.104
#&gt; SRR1617462     2   0.673    0.59660 0.096 0.704 0.096 0.104
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     4  0.0932     0.5778 0.004 0.020 0.004 0.972 0.000
#&gt; SRR1617431     4  0.1093     0.5778 0.004 0.020 0.004 0.968 0.004
#&gt; SRR1617410     1  0.5582    -0.5663 0.568 0.024 0.036 0.000 0.372
#&gt; SRR1617411     1  0.5582    -0.5663 0.568 0.024 0.036 0.000 0.372
#&gt; SRR1617412     3  0.6143     0.7214 0.292 0.032 0.604 0.008 0.064
#&gt; SRR1617413     3  0.6143     0.7214 0.292 0.032 0.604 0.008 0.064
#&gt; SRR1617414     5  0.5573     0.9972 0.460 0.024 0.028 0.000 0.488
#&gt; SRR1617415     5  0.5573     0.9972 0.460 0.024 0.028 0.000 0.488
#&gt; SRR1617416     1  0.4299     0.4807 0.784 0.016 0.160 0.004 0.036
#&gt; SRR1617417     1  0.4299     0.4807 0.784 0.016 0.160 0.004 0.036
#&gt; SRR1617418     3  0.5314     0.7212 0.280 0.016 0.652 0.000 0.052
#&gt; SRR1617419     3  0.5314     0.7212 0.280 0.016 0.652 0.000 0.052
#&gt; SRR1617420     1  0.5282    -0.7291 0.524 0.008 0.024 0.004 0.440
#&gt; SRR1617421     1  0.5282    -0.7291 0.524 0.008 0.024 0.004 0.440
#&gt; SRR1617422     1  0.0510     0.5631 0.984 0.000 0.000 0.000 0.016
#&gt; SRR1617423     1  0.0510     0.5631 0.984 0.000 0.000 0.000 0.016
#&gt; SRR1617424     1  0.0290     0.5666 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1617425     1  0.0290     0.5666 0.992 0.000 0.000 0.000 0.008
#&gt; SRR1617427     1  0.2305     0.4855 0.896 0.000 0.012 0.000 0.092
#&gt; SRR1617426     1  0.2305     0.4855 0.896 0.000 0.012 0.000 0.092
#&gt; SRR1617428     1  0.7912    -0.0739 0.476 0.052 0.288 0.040 0.144
#&gt; SRR1617429     1  0.7912    -0.0739 0.476 0.052 0.288 0.040 0.144
#&gt; SRR1617432     5  0.5573     0.9972 0.460 0.028 0.024 0.000 0.488
#&gt; SRR1617433     5  0.5573     0.9972 0.460 0.028 0.024 0.000 0.488
#&gt; SRR1617434     1  0.5320    -0.7413 0.524 0.016 0.024 0.000 0.436
#&gt; SRR1617436     3  0.7131     0.5535 0.304 0.020 0.512 0.024 0.140
#&gt; SRR1617435     1  0.5320    -0.7413 0.524 0.016 0.024 0.000 0.436
#&gt; SRR1617437     3  0.7131     0.5535 0.304 0.020 0.512 0.024 0.140
#&gt; SRR1617438     3  0.5088     0.7360 0.340 0.024 0.620 0.000 0.016
#&gt; SRR1617439     3  0.5088     0.7360 0.340 0.024 0.620 0.000 0.016
#&gt; SRR1617440     3  0.5338     0.7210 0.328 0.060 0.608 0.000 0.004
#&gt; SRR1617441     3  0.5338     0.7210 0.328 0.060 0.608 0.000 0.004
#&gt; SRR1617443     3  0.5307     0.7334 0.332 0.024 0.616 0.000 0.028
#&gt; SRR1617442     3  0.5307     0.7334 0.332 0.024 0.616 0.000 0.028
#&gt; SRR1617444     1  0.3416     0.5025 0.840 0.020 0.124 0.000 0.016
#&gt; SRR1617445     1  0.3416     0.5025 0.840 0.020 0.124 0.000 0.016
#&gt; SRR1617446     1  0.0898     0.5738 0.972 0.000 0.020 0.000 0.008
#&gt; SRR1617447     1  0.0898     0.5738 0.972 0.000 0.020 0.000 0.008
#&gt; SRR1617448     1  0.1251     0.5739 0.956 0.000 0.036 0.000 0.008
#&gt; SRR1617449     1  0.1251     0.5739 0.956 0.000 0.036 0.000 0.008
#&gt; SRR1617451     4  0.7258     0.3757 0.020 0.420 0.052 0.424 0.084
#&gt; SRR1617450     4  0.7258     0.3757 0.020 0.420 0.052 0.424 0.084
#&gt; SRR1617452     3  0.8363     0.1880 0.300 0.196 0.372 0.008 0.124
#&gt; SRR1617454     2  0.7254    -0.3566 0.020 0.456 0.028 0.368 0.128
#&gt; SRR1617453     3  0.8363     0.1880 0.300 0.196 0.372 0.008 0.124
#&gt; SRR1617456     2  0.4795     0.5143 0.120 0.752 0.116 0.000 0.012
#&gt; SRR1617457     2  0.4795     0.5143 0.120 0.752 0.116 0.000 0.012
#&gt; SRR1617455     2  0.7254    -0.3566 0.020 0.456 0.028 0.368 0.128
#&gt; SRR1617458     2  0.4894     0.5148 0.120 0.748 0.116 0.000 0.016
#&gt; SRR1617459     2  0.4894     0.5148 0.120 0.748 0.116 0.000 0.016
#&gt; SRR1617460     2  0.7585     0.2320 0.380 0.388 0.156 0.000 0.076
#&gt; SRR1617461     2  0.7585     0.2320 0.380 0.388 0.156 0.000 0.076
#&gt; SRR1617463     2  0.7400     0.1866 0.068 0.592 0.040 0.152 0.148
#&gt; SRR1617462     2  0.7400     0.1866 0.068 0.592 0.040 0.152 0.148
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3 p4    p5    p6
#&gt; SRR1617430     2   0.279      0.563 0.000 0.864 0.004 NA 0.020 0.008
#&gt; SRR1617431     2   0.275      0.563 0.000 0.868 0.004 NA 0.020 0.008
#&gt; SRR1617410     5   0.509      0.787 0.364 0.004 0.012 NA 0.580 0.012
#&gt; SRR1617411     5   0.509      0.787 0.364 0.004 0.012 NA 0.580 0.012
#&gt; SRR1617412     3   0.600      0.658 0.180 0.000 0.604 NA 0.036 0.008
#&gt; SRR1617413     3   0.600      0.658 0.180 0.000 0.604 NA 0.036 0.008
#&gt; SRR1617414     5   0.492      0.839 0.276 0.000 0.000 NA 0.632 0.004
#&gt; SRR1617415     5   0.492      0.839 0.276 0.000 0.000 NA 0.632 0.004
#&gt; SRR1617416     1   0.456      0.660 0.768 0.004 0.120 NA 0.044 0.008
#&gt; SRR1617417     1   0.456      0.660 0.768 0.004 0.120 NA 0.044 0.008
#&gt; SRR1617418     3   0.510      0.686 0.200 0.000 0.664 NA 0.004 0.008
#&gt; SRR1617419     3   0.510      0.686 0.200 0.000 0.664 NA 0.004 0.008
#&gt; SRR1617420     5   0.547      0.818 0.332 0.004 0.032 NA 0.584 0.008
#&gt; SRR1617421     5   0.547      0.818 0.332 0.004 0.032 NA 0.584 0.008
#&gt; SRR1617422     1   0.144      0.764 0.944 0.000 0.000 NA 0.040 0.004
#&gt; SRR1617423     1   0.144      0.764 0.944 0.000 0.000 NA 0.040 0.004
#&gt; SRR1617424     1   0.151      0.763 0.944 0.000 0.004 NA 0.036 0.004
#&gt; SRR1617425     1   0.151      0.763 0.944 0.000 0.004 NA 0.036 0.004
#&gt; SRR1617427     1   0.329      0.689 0.840 0.000 0.016 NA 0.104 0.004
#&gt; SRR1617426     1   0.329      0.689 0.840 0.000 0.016 NA 0.104 0.004
#&gt; SRR1617428     1   0.742      0.152 0.440 0.028 0.104 NA 0.040 0.044
#&gt; SRR1617429     1   0.742      0.152 0.440 0.028 0.104 NA 0.040 0.044
#&gt; SRR1617432     5   0.490      0.840 0.272 0.000 0.000 NA 0.636 0.004
#&gt; SRR1617433     5   0.490      0.840 0.272 0.000 0.000 NA 0.636 0.004
#&gt; SRR1617434     5   0.432      0.850 0.324 0.000 0.016 NA 0.648 0.004
#&gt; SRR1617436     3   0.712      0.493 0.244 0.008 0.460 NA 0.056 0.008
#&gt; SRR1617435     5   0.432      0.850 0.324 0.000 0.016 NA 0.648 0.004
#&gt; SRR1617437     3   0.712      0.493 0.244 0.008 0.460 NA 0.056 0.008
#&gt; SRR1617438     3   0.449      0.700 0.236 0.000 0.708 NA 0.016 0.028
#&gt; SRR1617439     3   0.449      0.700 0.236 0.000 0.708 NA 0.016 0.028
#&gt; SRR1617440     3   0.517      0.679 0.240 0.000 0.656 NA 0.016 0.080
#&gt; SRR1617441     3   0.517      0.679 0.240 0.000 0.656 NA 0.016 0.080
#&gt; SRR1617443     3   0.459      0.696 0.228 0.000 0.708 NA 0.032 0.020
#&gt; SRR1617442     3   0.459      0.696 0.228 0.000 0.708 NA 0.032 0.020
#&gt; SRR1617444     1   0.291      0.723 0.864 0.000 0.096 NA 0.008 0.020
#&gt; SRR1617445     1   0.291      0.723 0.864 0.000 0.096 NA 0.008 0.020
#&gt; SRR1617446     1   0.213      0.775 0.920 0.000 0.028 NA 0.016 0.008
#&gt; SRR1617447     1   0.213      0.775 0.920 0.000 0.028 NA 0.016 0.008
#&gt; SRR1617448     1   0.201      0.776 0.924 0.000 0.032 NA 0.008 0.008
#&gt; SRR1617449     1   0.201      0.776 0.924 0.000 0.032 NA 0.008 0.008
#&gt; SRR1617451     2   0.599      0.351 0.020 0.524 0.032 NA 0.024 0.376
#&gt; SRR1617450     2   0.599      0.351 0.020 0.524 0.032 NA 0.024 0.376
#&gt; SRR1617452     3   0.863      0.114 0.276 0.004 0.284 NA 0.108 0.224
#&gt; SRR1617454     6   0.644     -0.226 0.016 0.400 0.024 NA 0.028 0.468
#&gt; SRR1617453     3   0.863      0.114 0.276 0.004 0.284 NA 0.108 0.224
#&gt; SRR1617456     6   0.370      0.503 0.096 0.000 0.116 NA 0.000 0.788
#&gt; SRR1617457     6   0.370      0.503 0.096 0.000 0.116 NA 0.000 0.788
#&gt; SRR1617455     6   0.644     -0.226 0.016 0.400 0.024 NA 0.028 0.468
#&gt; SRR1617458     6   0.397      0.504 0.100 0.000 0.124 NA 0.000 0.772
#&gt; SRR1617459     6   0.397      0.504 0.100 0.000 0.124 NA 0.000 0.772
#&gt; SRR1617460     6   0.716      0.262 0.388 0.004 0.112 NA 0.052 0.404
#&gt; SRR1617461     6   0.716      0.262 0.388 0.004 0.112 NA 0.052 0.404
#&gt; SRR1617463     6   0.723      0.247 0.088 0.184 0.036 NA 0.032 0.568
#&gt; SRR1617462     6   0.723      0.247 0.088 0.184 0.036 NA 0.032 0.568
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.925           0.947       0.978         0.4934 0.508   0.508
#> 3 3 0.771           0.890       0.932         0.3440 0.765   0.566
#> 4 4 0.698           0.820       0.838         0.1323 0.902   0.713
#> 5 5 0.834           0.739       0.867         0.0755 0.927   0.717
#> 6 6 0.795           0.671       0.804         0.0361 0.947   0.752
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617431     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617410     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617411     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617412     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617413     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617414     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617415     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617416     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617417     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617418     2  0.0672      0.970 0.008 0.992
#&gt; SRR1617419     2  0.0672      0.970 0.008 0.992
#&gt; SRR1617420     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617421     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617422     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617423     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617424     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617425     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617427     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617426     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617428     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617429     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617432     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617433     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617434     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617436     2  0.7815      0.705 0.232 0.768
#&gt; SRR1617435     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617437     2  0.7815      0.705 0.232 0.768
#&gt; SRR1617438     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617439     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617440     1  0.9286      0.486 0.656 0.344
#&gt; SRR1617441     1  0.9286      0.486 0.656 0.344
#&gt; SRR1617443     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617442     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617444     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617445     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617446     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617447     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617448     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617449     1  0.0000      0.977 1.000 0.000
#&gt; SRR1617451     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617450     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617452     2  0.1184      0.963 0.016 0.984
#&gt; SRR1617454     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617453     2  0.1184      0.963 0.016 0.984
#&gt; SRR1617456     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617457     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617455     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617458     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617459     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617460     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617461     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617463     2  0.0000      0.974 0.000 1.000
#&gt; SRR1617462     2  0.0000      0.974 0.000 1.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.1031      0.900 0.000 0.976 0.024
#&gt; SRR1617431     2  0.1031      0.900 0.000 0.976 0.024
#&gt; SRR1617410     1  0.0892      0.930 0.980 0.000 0.020
#&gt; SRR1617411     1  0.0892      0.930 0.980 0.000 0.020
#&gt; SRR1617412     3  0.0424      0.928 0.008 0.000 0.992
#&gt; SRR1617413     3  0.0424      0.928 0.008 0.000 0.992
#&gt; SRR1617414     1  0.2165      0.921 0.936 0.000 0.064
#&gt; SRR1617415     1  0.2165      0.921 0.936 0.000 0.064
#&gt; SRR1617416     1  0.3879      0.831 0.848 0.000 0.152
#&gt; SRR1617417     1  0.3879      0.831 0.848 0.000 0.152
#&gt; SRR1617418     3  0.3816      0.855 0.000 0.148 0.852
#&gt; SRR1617419     3  0.3816      0.855 0.000 0.148 0.852
#&gt; SRR1617420     1  0.2165      0.921 0.936 0.000 0.064
#&gt; SRR1617421     1  0.2165      0.921 0.936 0.000 0.064
#&gt; SRR1617422     1  0.0592      0.932 0.988 0.000 0.012
#&gt; SRR1617423     1  0.0592      0.932 0.988 0.000 0.012
#&gt; SRR1617424     1  0.0592      0.932 0.988 0.000 0.012
#&gt; SRR1617425     1  0.0592      0.932 0.988 0.000 0.012
#&gt; SRR1617427     1  0.0592      0.932 0.988 0.000 0.012
#&gt; SRR1617426     1  0.0592      0.932 0.988 0.000 0.012
#&gt; SRR1617428     2  0.4834      0.726 0.004 0.792 0.204
#&gt; SRR1617429     2  0.4834      0.726 0.004 0.792 0.204
#&gt; SRR1617432     1  0.2165      0.921 0.936 0.000 0.064
#&gt; SRR1617433     1  0.2165      0.921 0.936 0.000 0.064
#&gt; SRR1617434     1  0.1860      0.925 0.948 0.000 0.052
#&gt; SRR1617436     3  0.4033      0.860 0.008 0.136 0.856
#&gt; SRR1617435     1  0.1860      0.925 0.948 0.000 0.052
#&gt; SRR1617437     3  0.4033      0.860 0.008 0.136 0.856
#&gt; SRR1617438     3  0.0892      0.930 0.020 0.000 0.980
#&gt; SRR1617439     3  0.0892      0.930 0.020 0.000 0.980
#&gt; SRR1617440     3  0.1399      0.928 0.028 0.004 0.968
#&gt; SRR1617441     3  0.1399      0.928 0.028 0.004 0.968
#&gt; SRR1617443     3  0.1289      0.925 0.032 0.000 0.968
#&gt; SRR1617442     3  0.1289      0.925 0.032 0.000 0.968
#&gt; SRR1617444     1  0.5591      0.615 0.696 0.000 0.304
#&gt; SRR1617445     1  0.5591      0.615 0.696 0.000 0.304
#&gt; SRR1617446     1  0.0592      0.932 0.988 0.000 0.012
#&gt; SRR1617447     1  0.0592      0.932 0.988 0.000 0.012
#&gt; SRR1617448     1  0.0892      0.930 0.980 0.000 0.020
#&gt; SRR1617449     1  0.0892      0.930 0.980 0.000 0.020
#&gt; SRR1617451     2  0.0000      0.908 0.000 1.000 0.000
#&gt; SRR1617450     2  0.0000      0.908 0.000 1.000 0.000
#&gt; SRR1617452     3  0.1950      0.908 0.008 0.040 0.952
#&gt; SRR1617454     2  0.0000      0.908 0.000 1.000 0.000
#&gt; SRR1617453     3  0.1950      0.908 0.008 0.040 0.952
#&gt; SRR1617456     2  0.3412      0.882 0.000 0.876 0.124
#&gt; SRR1617457     2  0.3412      0.882 0.000 0.876 0.124
#&gt; SRR1617455     2  0.0000      0.908 0.000 1.000 0.000
#&gt; SRR1617458     2  0.3412      0.882 0.000 0.876 0.124
#&gt; SRR1617459     2  0.3412      0.882 0.000 0.876 0.124
#&gt; SRR1617460     2  0.3896      0.875 0.008 0.864 0.128
#&gt; SRR1617461     2  0.3896      0.875 0.008 0.864 0.128
#&gt; SRR1617463     2  0.0000      0.908 0.000 1.000 0.000
#&gt; SRR1617462     2  0.0000      0.908 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.5982      0.673 0.112 0.684 0.204 0.000
#&gt; SRR1617431     2  0.5982      0.673 0.112 0.684 0.204 0.000
#&gt; SRR1617410     1  0.3610      0.989 0.800 0.000 0.000 0.200
#&gt; SRR1617411     1  0.3610      0.989 0.800 0.000 0.000 0.200
#&gt; SRR1617412     3  0.3439      0.824 0.048 0.000 0.868 0.084
#&gt; SRR1617413     3  0.3439      0.824 0.048 0.000 0.868 0.084
#&gt; SRR1617414     1  0.3569      0.992 0.804 0.000 0.000 0.196
#&gt; SRR1617415     1  0.3569      0.992 0.804 0.000 0.000 0.196
#&gt; SRR1617416     4  0.2376      0.833 0.016 0.000 0.068 0.916
#&gt; SRR1617417     4  0.2376      0.833 0.016 0.000 0.068 0.916
#&gt; SRR1617418     3  0.3236      0.730 0.088 0.028 0.880 0.004
#&gt; SRR1617419     3  0.3236      0.730 0.088 0.028 0.880 0.004
#&gt; SRR1617420     1  0.3969      0.974 0.804 0.000 0.016 0.180
#&gt; SRR1617421     1  0.3969      0.974 0.804 0.000 0.016 0.180
#&gt; SRR1617422     4  0.2408      0.862 0.104 0.000 0.000 0.896
#&gt; SRR1617423     4  0.2408      0.862 0.104 0.000 0.000 0.896
#&gt; SRR1617424     4  0.2345      0.865 0.100 0.000 0.000 0.900
#&gt; SRR1617425     4  0.2345      0.865 0.100 0.000 0.000 0.900
#&gt; SRR1617427     4  0.2973      0.823 0.144 0.000 0.000 0.856
#&gt; SRR1617426     4  0.2973      0.823 0.144 0.000 0.000 0.856
#&gt; SRR1617428     2  0.7152      0.423 0.124 0.512 0.360 0.004
#&gt; SRR1617429     2  0.7152      0.423 0.124 0.512 0.360 0.004
#&gt; SRR1617432     1  0.3569      0.992 0.804 0.000 0.000 0.196
#&gt; SRR1617433     1  0.3569      0.992 0.804 0.000 0.000 0.196
#&gt; SRR1617434     1  0.3569      0.992 0.804 0.000 0.000 0.196
#&gt; SRR1617436     3  0.3934      0.695 0.116 0.048 0.836 0.000
#&gt; SRR1617435     1  0.3569      0.992 0.804 0.000 0.000 0.196
#&gt; SRR1617437     3  0.3934      0.695 0.116 0.048 0.836 0.000
#&gt; SRR1617438     3  0.4706      0.823 0.072 0.000 0.788 0.140
#&gt; SRR1617439     3  0.4706      0.823 0.072 0.000 0.788 0.140
#&gt; SRR1617440     3  0.5157      0.821 0.068 0.020 0.784 0.128
#&gt; SRR1617441     3  0.5157      0.821 0.068 0.020 0.784 0.128
#&gt; SRR1617443     3  0.4841      0.821 0.080 0.000 0.780 0.140
#&gt; SRR1617442     3  0.4841      0.821 0.080 0.000 0.780 0.140
#&gt; SRR1617444     4  0.3146      0.805 0.036 0.008 0.064 0.892
#&gt; SRR1617445     4  0.3146      0.805 0.036 0.008 0.064 0.892
#&gt; SRR1617446     4  0.1118      0.883 0.036 0.000 0.000 0.964
#&gt; SRR1617447     4  0.1118      0.883 0.036 0.000 0.000 0.964
#&gt; SRR1617448     4  0.0188      0.878 0.004 0.000 0.000 0.996
#&gt; SRR1617449     4  0.0188      0.878 0.004 0.000 0.000 0.996
#&gt; SRR1617451     2  0.3521      0.787 0.052 0.864 0.084 0.000
#&gt; SRR1617450     2  0.3521      0.787 0.052 0.864 0.084 0.000
#&gt; SRR1617452     3  0.7275      0.628 0.176 0.136 0.640 0.048
#&gt; SRR1617454     2  0.1174      0.818 0.012 0.968 0.020 0.000
#&gt; SRR1617453     3  0.7275      0.628 0.176 0.136 0.640 0.048
#&gt; SRR1617456     2  0.3370      0.799 0.080 0.872 0.048 0.000
#&gt; SRR1617457     2  0.3370      0.799 0.080 0.872 0.048 0.000
#&gt; SRR1617455     2  0.1174      0.818 0.012 0.968 0.020 0.000
#&gt; SRR1617458     2  0.3370      0.799 0.080 0.872 0.048 0.000
#&gt; SRR1617459     2  0.3370      0.799 0.080 0.872 0.048 0.000
#&gt; SRR1617460     2  0.3902      0.793 0.080 0.860 0.036 0.024
#&gt; SRR1617461     2  0.3902      0.793 0.080 0.860 0.036 0.024
#&gt; SRR1617463     2  0.0469      0.820 0.000 0.988 0.012 0.000
#&gt; SRR1617462     2  0.0469      0.820 0.000 0.988 0.012 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     4  0.2124     0.7697 0.000 0.096 0.004 0.900 0.000
#&gt; SRR1617431     4  0.2124     0.7697 0.000 0.096 0.004 0.900 0.000
#&gt; SRR1617410     5  0.0703     0.9752 0.024 0.000 0.000 0.000 0.976
#&gt; SRR1617411     5  0.0703     0.9752 0.024 0.000 0.000 0.000 0.976
#&gt; SRR1617412     3  0.2017     0.7895 0.000 0.000 0.912 0.080 0.008
#&gt; SRR1617413     3  0.2017     0.7895 0.000 0.000 0.912 0.080 0.008
#&gt; SRR1617414     5  0.0162     0.9915 0.000 0.000 0.000 0.004 0.996
#&gt; SRR1617415     5  0.0162     0.9915 0.000 0.000 0.000 0.004 0.996
#&gt; SRR1617416     1  0.4797     0.7445 0.756 0.004 0.168 0.040 0.032
#&gt; SRR1617417     1  0.4797     0.7445 0.756 0.004 0.168 0.040 0.032
#&gt; SRR1617418     3  0.3966     0.6237 0.000 0.000 0.664 0.336 0.000
#&gt; SRR1617419     3  0.3966     0.6237 0.000 0.000 0.664 0.336 0.000
#&gt; SRR1617420     5  0.0162     0.9906 0.004 0.000 0.000 0.000 0.996
#&gt; SRR1617421     5  0.0162     0.9906 0.004 0.000 0.000 0.000 0.996
#&gt; SRR1617422     1  0.0955     0.9368 0.968 0.000 0.000 0.004 0.028
#&gt; SRR1617423     1  0.0955     0.9368 0.968 0.000 0.000 0.004 0.028
#&gt; SRR1617424     1  0.0771     0.9387 0.976 0.000 0.000 0.004 0.020
#&gt; SRR1617425     1  0.0771     0.9387 0.976 0.000 0.000 0.004 0.020
#&gt; SRR1617427     1  0.1768     0.9116 0.924 0.000 0.000 0.004 0.072
#&gt; SRR1617426     1  0.1768     0.9116 0.924 0.000 0.000 0.004 0.072
#&gt; SRR1617428     4  0.1652     0.7274 0.004 0.004 0.040 0.944 0.008
#&gt; SRR1617429     4  0.1652     0.7274 0.004 0.004 0.040 0.944 0.008
#&gt; SRR1617432     5  0.0162     0.9915 0.000 0.000 0.000 0.004 0.996
#&gt; SRR1617433     5  0.0162     0.9915 0.000 0.000 0.000 0.004 0.996
#&gt; SRR1617434     5  0.0000     0.9912 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617436     3  0.4443     0.4347 0.000 0.000 0.524 0.472 0.004
#&gt; SRR1617435     5  0.0000     0.9912 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617437     3  0.4443     0.4347 0.000 0.000 0.524 0.472 0.004
#&gt; SRR1617438     3  0.1117     0.8043 0.020 0.000 0.964 0.000 0.016
#&gt; SRR1617439     3  0.1117     0.8043 0.020 0.000 0.964 0.000 0.016
#&gt; SRR1617440     3  0.2217     0.7891 0.024 0.044 0.920 0.000 0.012
#&gt; SRR1617441     3  0.2217     0.7891 0.024 0.044 0.920 0.000 0.012
#&gt; SRR1617443     3  0.1564     0.8014 0.024 0.004 0.948 0.000 0.024
#&gt; SRR1617442     3  0.1564     0.8014 0.024 0.004 0.948 0.000 0.024
#&gt; SRR1617444     1  0.1106     0.9305 0.964 0.024 0.012 0.000 0.000
#&gt; SRR1617445     1  0.1106     0.9305 0.964 0.024 0.012 0.000 0.000
#&gt; SRR1617446     1  0.0579     0.9395 0.984 0.008 0.000 0.000 0.008
#&gt; SRR1617447     1  0.0579     0.9395 0.984 0.008 0.000 0.000 0.008
#&gt; SRR1617448     1  0.0451     0.9386 0.988 0.008 0.000 0.000 0.004
#&gt; SRR1617449     1  0.0451     0.9386 0.988 0.008 0.000 0.000 0.004
#&gt; SRR1617451     4  0.4066     0.5261 0.004 0.324 0.000 0.672 0.000
#&gt; SRR1617450     4  0.4066     0.5261 0.004 0.324 0.000 0.672 0.000
#&gt; SRR1617452     2  0.6294     0.3434 0.004 0.560 0.296 0.132 0.008
#&gt; SRR1617454     2  0.4449    -0.0774 0.004 0.512 0.000 0.484 0.000
#&gt; SRR1617453     2  0.6294     0.3434 0.004 0.560 0.296 0.132 0.008
#&gt; SRR1617456     2  0.1012     0.6493 0.000 0.968 0.012 0.020 0.000
#&gt; SRR1617457     2  0.1012     0.6493 0.000 0.968 0.012 0.020 0.000
#&gt; SRR1617455     2  0.4449    -0.0774 0.004 0.512 0.000 0.484 0.000
#&gt; SRR1617458     2  0.1018     0.6508 0.000 0.968 0.016 0.016 0.000
#&gt; SRR1617459     2  0.1018     0.6508 0.000 0.968 0.016 0.016 0.000
#&gt; SRR1617460     2  0.1557     0.6352 0.000 0.940 0.008 0.052 0.000
#&gt; SRR1617461     2  0.1557     0.6352 0.000 0.940 0.008 0.052 0.000
#&gt; SRR1617463     2  0.4470     0.2131 0.004 0.596 0.004 0.396 0.000
#&gt; SRR1617462     2  0.4470     0.2131 0.004 0.596 0.004 0.396 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.2544      0.362 0.000 0.864 0.004 0.120 0.000 0.012
#&gt; SRR1617431     2  0.2544      0.362 0.000 0.864 0.004 0.120 0.000 0.012
#&gt; SRR1617410     5  0.1511      0.934 0.012 0.000 0.004 0.044 0.940 0.000
#&gt; SRR1617411     5  0.1511      0.934 0.012 0.000 0.004 0.044 0.940 0.000
#&gt; SRR1617412     3  0.3558      0.535 0.000 0.016 0.736 0.248 0.000 0.000
#&gt; SRR1617413     3  0.3558      0.535 0.000 0.016 0.736 0.248 0.000 0.000
#&gt; SRR1617414     5  0.1444      0.944 0.000 0.000 0.000 0.072 0.928 0.000
#&gt; SRR1617415     5  0.1444      0.944 0.000 0.000 0.000 0.072 0.928 0.000
#&gt; SRR1617416     1  0.6508      0.498 0.540 0.000 0.224 0.176 0.052 0.008
#&gt; SRR1617417     1  0.6508      0.498 0.540 0.000 0.224 0.176 0.052 0.008
#&gt; SRR1617418     3  0.5847     -0.363 0.000 0.196 0.444 0.360 0.000 0.000
#&gt; SRR1617419     3  0.5847     -0.363 0.000 0.196 0.444 0.360 0.000 0.000
#&gt; SRR1617420     5  0.0632      0.948 0.000 0.000 0.000 0.024 0.976 0.000
#&gt; SRR1617421     5  0.0632      0.948 0.000 0.000 0.000 0.024 0.976 0.000
#&gt; SRR1617422     1  0.2300      0.861 0.856 0.000 0.000 0.144 0.000 0.000
#&gt; SRR1617423     1  0.2300      0.861 0.856 0.000 0.000 0.144 0.000 0.000
#&gt; SRR1617424     1  0.2260      0.861 0.860 0.000 0.000 0.140 0.000 0.000
#&gt; SRR1617425     1  0.2260      0.861 0.860 0.000 0.000 0.140 0.000 0.000
#&gt; SRR1617427     1  0.3312      0.835 0.792 0.000 0.000 0.180 0.028 0.000
#&gt; SRR1617426     1  0.3312      0.835 0.792 0.000 0.000 0.180 0.028 0.000
#&gt; SRR1617428     2  0.4357     -0.246 0.000 0.608 0.004 0.368 0.004 0.016
#&gt; SRR1617429     2  0.4357     -0.246 0.000 0.608 0.004 0.368 0.004 0.016
#&gt; SRR1617432     5  0.1444      0.944 0.000 0.000 0.000 0.072 0.928 0.000
#&gt; SRR1617433     5  0.1444      0.944 0.000 0.000 0.000 0.072 0.928 0.000
#&gt; SRR1617434     5  0.0547      0.950 0.000 0.000 0.000 0.020 0.980 0.000
#&gt; SRR1617436     4  0.5888      1.000 0.000 0.320 0.220 0.460 0.000 0.000
#&gt; SRR1617435     5  0.0547      0.950 0.000 0.000 0.000 0.020 0.980 0.000
#&gt; SRR1617437     4  0.5888      1.000 0.000 0.320 0.220 0.460 0.000 0.000
#&gt; SRR1617438     3  0.0291      0.714 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; SRR1617439     3  0.0291      0.714 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; SRR1617440     3  0.2355      0.657 0.000 0.004 0.876 0.008 0.000 0.112
#&gt; SRR1617441     3  0.2355      0.657 0.000 0.004 0.876 0.008 0.000 0.112
#&gt; SRR1617443     3  0.0291      0.714 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; SRR1617442     3  0.0291      0.714 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; SRR1617444     1  0.1401      0.851 0.948 0.000 0.028 0.020 0.000 0.004
#&gt; SRR1617445     1  0.1401      0.851 0.948 0.000 0.028 0.020 0.000 0.004
#&gt; SRR1617446     1  0.0146      0.865 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; SRR1617447     1  0.0146      0.865 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; SRR1617448     1  0.0291      0.864 0.992 0.000 0.004 0.004 0.000 0.000
#&gt; SRR1617449     1  0.0291      0.864 0.992 0.000 0.004 0.004 0.000 0.000
#&gt; SRR1617451     2  0.2730      0.617 0.000 0.808 0.000 0.000 0.000 0.192
#&gt; SRR1617450     2  0.2730      0.617 0.000 0.808 0.000 0.000 0.000 0.192
#&gt; SRR1617452     6  0.6237      0.566 0.004 0.020 0.164 0.288 0.004 0.520
#&gt; SRR1617454     2  0.4389      0.546 0.000 0.660 0.000 0.052 0.000 0.288
#&gt; SRR1617453     6  0.6237      0.566 0.004 0.020 0.164 0.288 0.004 0.520
#&gt; SRR1617456     6  0.0713      0.762 0.000 0.028 0.000 0.000 0.000 0.972
#&gt; SRR1617457     6  0.0713      0.762 0.000 0.028 0.000 0.000 0.000 0.972
#&gt; SRR1617455     2  0.4389      0.546 0.000 0.660 0.000 0.052 0.000 0.288
#&gt; SRR1617458     6  0.0547      0.766 0.000 0.020 0.000 0.000 0.000 0.980
#&gt; SRR1617459     6  0.0547      0.766 0.000 0.020 0.000 0.000 0.000 0.980
#&gt; SRR1617460     6  0.3819      0.711 0.004 0.040 0.000 0.200 0.000 0.756
#&gt; SRR1617461     6  0.3819      0.711 0.004 0.040 0.000 0.200 0.000 0.756
#&gt; SRR1617463     2  0.5303      0.419 0.000 0.548 0.000 0.120 0.000 0.332
#&gt; SRR1617462     2  0.5303      0.419 0.000 0.548 0.000 0.120 0.000 0.332
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:pam






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.665           0.904       0.947         0.2366 0.743   0.743
#> 3 3 0.746           0.866       0.948         0.3337 0.992   0.989
#> 4 4 0.481           0.752       0.855         0.4810 0.871   0.826
#> 5 5 0.652           0.681       0.811         0.3212 0.776   0.638
#> 6 6 0.600           0.513       0.729         0.0744 0.734   0.434
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2   0.000      0.745 0.000 1.000
#&gt; SRR1617431     2   0.000      0.745 0.000 1.000
#&gt; SRR1617410     1   0.000      0.963 1.000 0.000
#&gt; SRR1617411     1   0.000      0.963 1.000 0.000
#&gt; SRR1617412     1   0.000      0.963 1.000 0.000
#&gt; SRR1617413     1   0.000      0.963 1.000 0.000
#&gt; SRR1617414     1   0.000      0.963 1.000 0.000
#&gt; SRR1617415     1   0.000      0.963 1.000 0.000
#&gt; SRR1617416     1   0.000      0.963 1.000 0.000
#&gt; SRR1617417     1   0.000      0.963 1.000 0.000
#&gt; SRR1617418     1   0.000      0.963 1.000 0.000
#&gt; SRR1617419     1   0.000      0.963 1.000 0.000
#&gt; SRR1617420     1   0.000      0.963 1.000 0.000
#&gt; SRR1617421     1   0.000      0.963 1.000 0.000
#&gt; SRR1617422     1   0.000      0.963 1.000 0.000
#&gt; SRR1617423     1   0.000      0.963 1.000 0.000
#&gt; SRR1617424     1   0.000      0.963 1.000 0.000
#&gt; SRR1617425     1   0.000      0.963 1.000 0.000
#&gt; SRR1617427     1   0.000      0.963 1.000 0.000
#&gt; SRR1617426     1   0.000      0.963 1.000 0.000
#&gt; SRR1617428     1   0.000      0.963 1.000 0.000
#&gt; SRR1617429     1   0.000      0.963 1.000 0.000
#&gt; SRR1617432     1   0.000      0.963 1.000 0.000
#&gt; SRR1617433     1   0.000      0.963 1.000 0.000
#&gt; SRR1617434     1   0.000      0.963 1.000 0.000
#&gt; SRR1617436     1   0.000      0.963 1.000 0.000
#&gt; SRR1617435     1   0.000      0.963 1.000 0.000
#&gt; SRR1617437     1   0.000      0.963 1.000 0.000
#&gt; SRR1617438     1   0.000      0.963 1.000 0.000
#&gt; SRR1617439     1   0.000      0.963 1.000 0.000
#&gt; SRR1617440     1   0.000      0.963 1.000 0.000
#&gt; SRR1617441     1   0.000      0.963 1.000 0.000
#&gt; SRR1617443     1   0.000      0.963 1.000 0.000
#&gt; SRR1617442     1   0.000      0.963 1.000 0.000
#&gt; SRR1617444     1   0.000      0.963 1.000 0.000
#&gt; SRR1617445     1   0.000      0.963 1.000 0.000
#&gt; SRR1617446     1   0.000      0.963 1.000 0.000
#&gt; SRR1617447     1   0.000      0.963 1.000 0.000
#&gt; SRR1617448     1   0.000      0.963 1.000 0.000
#&gt; SRR1617449     1   0.000      0.963 1.000 0.000
#&gt; SRR1617451     2   0.958      0.651 0.380 0.620
#&gt; SRR1617450     2   0.949      0.675 0.368 0.632
#&gt; SRR1617452     1   0.000      0.963 1.000 0.000
#&gt; SRR1617454     2   0.760      0.850 0.220 0.780
#&gt; SRR1617453     1   0.000      0.963 1.000 0.000
#&gt; SRR1617456     1   0.738      0.678 0.792 0.208
#&gt; SRR1617457     1   0.738      0.678 0.792 0.208
#&gt; SRR1617455     2   0.760      0.850 0.220 0.780
#&gt; SRR1617458     1   0.738      0.678 0.792 0.208
#&gt; SRR1617459     1   0.738      0.678 0.792 0.208
#&gt; SRR1617460     1   0.738      0.678 0.792 0.208
#&gt; SRR1617461     1   0.738      0.678 0.792 0.208
#&gt; SRR1617463     2   0.760      0.850 0.220 0.780
#&gt; SRR1617462     2   0.760      0.850 0.220 0.780
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617431     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617410     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617412     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617413     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617414     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617415     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617416     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617417     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617418     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617419     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617420     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617421     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617422     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617428     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617429     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617432     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617433     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617434     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617436     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617435     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617437     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617438     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617439     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617440     1  0.0237      0.938 0.996 0.004 0.000
#&gt; SRR1617441     1  0.0237      0.938 0.996 0.004 0.000
#&gt; SRR1617443     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617442     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617444     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617445     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617446     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.941 1.000 0.000 0.000
#&gt; SRR1617451     2  0.3551      0.734 0.132 0.868 0.000
#&gt; SRR1617450     2  0.3267      0.761 0.116 0.884 0.000
#&gt; SRR1617452     1  0.0424      0.935 0.992 0.008 0.000
#&gt; SRR1617454     2  0.1289      0.849 0.000 0.968 0.032
#&gt; SRR1617453     1  0.0424      0.935 0.992 0.008 0.000
#&gt; SRR1617456     1  0.6154      0.343 0.592 0.408 0.000
#&gt; SRR1617457     1  0.6154      0.343 0.592 0.408 0.000
#&gt; SRR1617455     2  0.1289      0.849 0.000 0.968 0.032
#&gt; SRR1617458     1  0.6154      0.343 0.592 0.408 0.000
#&gt; SRR1617459     1  0.6154      0.343 0.592 0.408 0.000
#&gt; SRR1617460     1  0.5988      0.427 0.632 0.368 0.000
#&gt; SRR1617461     1  0.5988      0.427 0.632 0.368 0.000
#&gt; SRR1617463     2  0.2031      0.858 0.016 0.952 0.032
#&gt; SRR1617462     2  0.2031      0.858 0.016 0.952 0.032
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4
#&gt; SRR1617430     3  0.0000     1.0000 0.000 0.000  1 0.000
#&gt; SRR1617431     3  0.0000     1.0000 0.000 0.000  1 0.000
#&gt; SRR1617410     1  0.3311     0.7495 0.828 0.172  0 0.000
#&gt; SRR1617411     1  0.3311     0.7495 0.828 0.172  0 0.000
#&gt; SRR1617412     1  0.1637     0.8349 0.940 0.060  0 0.000
#&gt; SRR1617413     1  0.1716     0.8335 0.936 0.064  0 0.000
#&gt; SRR1617414     1  0.3907     0.7031 0.768 0.232  0 0.000
#&gt; SRR1617415     1  0.3907     0.7031 0.768 0.232  0 0.000
#&gt; SRR1617416     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617417     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617418     1  0.2281     0.8168 0.904 0.096  0 0.000
#&gt; SRR1617419     1  0.2345     0.8147 0.900 0.100  0 0.000
#&gt; SRR1617420     1  0.3907     0.7031 0.768 0.232  0 0.000
#&gt; SRR1617421     1  0.3907     0.7031 0.768 0.232  0 0.000
#&gt; SRR1617422     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617423     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617424     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617425     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617427     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617426     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617428     1  0.0469     0.8448 0.988 0.012  0 0.000
#&gt; SRR1617429     1  0.0707     0.8438 0.980 0.020  0 0.000
#&gt; SRR1617432     1  0.3907     0.7031 0.768 0.232  0 0.000
#&gt; SRR1617433     1  0.3907     0.7031 0.768 0.232  0 0.000
#&gt; SRR1617434     1  0.3907     0.7031 0.768 0.232  0 0.000
#&gt; SRR1617436     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617435     1  0.3907     0.7031 0.768 0.232  0 0.000
#&gt; SRR1617437     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617438     1  0.2647     0.8028 0.880 0.120  0 0.000
#&gt; SRR1617439     1  0.2647     0.8028 0.880 0.120  0 0.000
#&gt; SRR1617440     1  0.3942     0.6791 0.764 0.236  0 0.000
#&gt; SRR1617441     1  0.4250     0.6138 0.724 0.276  0 0.000
#&gt; SRR1617443     1  0.2647     0.8028 0.880 0.120  0 0.000
#&gt; SRR1617442     1  0.2647     0.8028 0.880 0.120  0 0.000
#&gt; SRR1617444     1  0.1637     0.8349 0.940 0.060  0 0.000
#&gt; SRR1617445     1  0.1637     0.8349 0.940 0.060  0 0.000
#&gt; SRR1617446     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617447     1  0.0000     0.8458 1.000 0.000  0 0.000
#&gt; SRR1617448     1  0.1637     0.8349 0.940 0.060  0 0.000
#&gt; SRR1617449     1  0.1637     0.8349 0.940 0.060  0 0.000
#&gt; SRR1617451     2  0.5294    -0.0803 0.008 0.508  0 0.484
#&gt; SRR1617450     2  0.5294    -0.0803 0.008 0.508  0 0.484
#&gt; SRR1617452     1  0.3837     0.6959 0.776 0.224  0 0.000
#&gt; SRR1617454     4  0.0000     1.0000 0.000 0.000  0 1.000
#&gt; SRR1617453     1  0.3764     0.7064 0.784 0.216  0 0.000
#&gt; SRR1617456     2  0.3907     0.7090 0.232 0.768  0 0.000
#&gt; SRR1617457     2  0.3907     0.7090 0.232 0.768  0 0.000
#&gt; SRR1617455     4  0.0000     1.0000 0.000 0.000  0 1.000
#&gt; SRR1617458     2  0.3907     0.7090 0.232 0.768  0 0.000
#&gt; SRR1617459     2  0.3907     0.7090 0.232 0.768  0 0.000
#&gt; SRR1617460     1  0.6179     0.2008 0.552 0.056  0 0.392
#&gt; SRR1617461     1  0.6179     0.2008 0.552 0.056  0 0.392
#&gt; SRR1617463     4  0.0000     1.0000 0.000 0.000  0 1.000
#&gt; SRR1617462     4  0.0000     1.0000 0.000 0.000  0 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3 p4    p5
#&gt; SRR1617430     4  0.0000      1.000 0.000 0.000 0.000  1 0.000
#&gt; SRR1617431     4  0.0000      1.000 0.000 0.000 0.000  1 0.000
#&gt; SRR1617410     1  0.3999      0.122 0.656 0.000 0.344  0 0.000
#&gt; SRR1617411     1  0.4045      0.066 0.644 0.000 0.356  0 0.000
#&gt; SRR1617412     3  0.0000      0.608 0.000 0.000 1.000  0 0.000
#&gt; SRR1617413     3  0.0000      0.608 0.000 0.000 1.000  0 0.000
#&gt; SRR1617414     1  0.0703      0.876 0.976 0.000 0.024  0 0.000
#&gt; SRR1617415     1  0.0703      0.876 0.976 0.000 0.024  0 0.000
#&gt; SRR1617416     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617417     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617418     3  0.2471      0.615 0.136 0.000 0.864  0 0.000
#&gt; SRR1617419     3  0.1544      0.612 0.068 0.000 0.932  0 0.000
#&gt; SRR1617420     1  0.0703      0.876 0.976 0.000 0.024  0 0.000
#&gt; SRR1617421     1  0.0703      0.876 0.976 0.000 0.024  0 0.000
#&gt; SRR1617422     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617423     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617424     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617425     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617427     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617426     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617428     3  0.4249      0.594 0.432 0.000 0.568  0 0.000
#&gt; SRR1617429     3  0.4201      0.604 0.408 0.000 0.592  0 0.000
#&gt; SRR1617432     1  0.0703      0.876 0.976 0.000 0.024  0 0.000
#&gt; SRR1617433     1  0.0703      0.876 0.976 0.000 0.024  0 0.000
#&gt; SRR1617434     1  0.0703      0.876 0.976 0.000 0.024  0 0.000
#&gt; SRR1617436     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617435     1  0.0703      0.876 0.976 0.000 0.024  0 0.000
#&gt; SRR1617437     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617438     3  0.0000      0.608 0.000 0.000 1.000  0 0.000
#&gt; SRR1617439     3  0.0000      0.608 0.000 0.000 1.000  0 0.000
#&gt; SRR1617440     3  0.0000      0.608 0.000 0.000 1.000  0 0.000
#&gt; SRR1617441     3  0.0000      0.608 0.000 0.000 1.000  0 0.000
#&gt; SRR1617443     3  0.0000      0.608 0.000 0.000 1.000  0 0.000
#&gt; SRR1617442     3  0.0000      0.608 0.000 0.000 1.000  0 0.000
#&gt; SRR1617444     3  0.4074      0.620 0.364 0.000 0.636  0 0.000
#&gt; SRR1617445     3  0.4074      0.620 0.364 0.000 0.636  0 0.000
#&gt; SRR1617446     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617447     3  0.4268      0.588 0.444 0.000 0.556  0 0.000
#&gt; SRR1617448     3  0.4074      0.620 0.364 0.000 0.636  0 0.000
#&gt; SRR1617449     3  0.4074      0.620 0.364 0.000 0.636  0 0.000
#&gt; SRR1617451     2  0.4735      0.631 0.024 0.664 0.008  0 0.304
#&gt; SRR1617450     2  0.4735      0.631 0.024 0.664 0.008  0 0.304
#&gt; SRR1617452     3  0.0451      0.610 0.008 0.004 0.988  0 0.000
#&gt; SRR1617454     5  0.0000      1.000 0.000 0.000 0.000  0 1.000
#&gt; SRR1617453     3  0.0671      0.612 0.016 0.004 0.980  0 0.000
#&gt; SRR1617456     2  0.0000      0.844 0.000 1.000 0.000  0 0.000
#&gt; SRR1617457     2  0.0000      0.844 0.000 1.000 0.000  0 0.000
#&gt; SRR1617455     5  0.0000      1.000 0.000 0.000 0.000  0 1.000
#&gt; SRR1617458     2  0.0000      0.844 0.000 1.000 0.000  0 0.000
#&gt; SRR1617459     2  0.0000      0.844 0.000 1.000 0.000  0 0.000
#&gt; SRR1617460     3  0.4150      0.439 0.000 0.000 0.612  0 0.388
#&gt; SRR1617461     3  0.4150      0.439 0.000 0.000 0.612  0 0.388
#&gt; SRR1617463     5  0.0000      1.000 0.000 0.000 0.000  0 1.000
#&gt; SRR1617462     5  0.0000      1.000 0.000 0.000 0.000  0 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     1  0.5114    -0.5140 0.468 0.000 0.000 0.452 0.080 0.000
#&gt; SRR1617431     1  0.5114    -0.5140 0.468 0.000 0.000 0.452 0.080 0.000
#&gt; SRR1617410     1  0.5431     0.4131 0.532 0.000 0.332 0.000 0.136 0.000
#&gt; SRR1617411     1  0.5367     0.4164 0.532 0.000 0.344 0.000 0.124 0.000
#&gt; SRR1617412     3  0.0000     0.6984 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617413     3  0.0000     0.6984 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617414     5  0.1556     1.0000 0.080 0.000 0.000 0.000 0.920 0.000
#&gt; SRR1617415     5  0.1556     1.0000 0.080 0.000 0.000 0.000 0.920 0.000
#&gt; SRR1617416     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617417     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617418     3  0.2219     0.5931 0.136 0.000 0.864 0.000 0.000 0.000
#&gt; SRR1617419     3  0.1387     0.6584 0.068 0.000 0.932 0.000 0.000 0.000
#&gt; SRR1617420     1  0.4083    -0.0534 0.532 0.000 0.008 0.000 0.460 0.000
#&gt; SRR1617421     1  0.4083    -0.0534 0.532 0.000 0.008 0.000 0.460 0.000
#&gt; SRR1617422     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617423     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617424     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617425     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617427     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617426     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617428     3  0.3868    -0.3959 0.496 0.000 0.504 0.000 0.000 0.000
#&gt; SRR1617429     3  0.3843    -0.2454 0.452 0.000 0.548 0.000 0.000 0.000
#&gt; SRR1617432     5  0.1556     1.0000 0.080 0.000 0.000 0.000 0.920 0.000
#&gt; SRR1617433     5  0.1556     1.0000 0.080 0.000 0.000 0.000 0.920 0.000
#&gt; SRR1617434     1  0.3989    -0.0757 0.528 0.000 0.004 0.000 0.468 0.000
#&gt; SRR1617436     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617435     1  0.3989    -0.0757 0.528 0.000 0.004 0.000 0.468 0.000
#&gt; SRR1617437     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617438     3  0.0000     0.6984 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617439     3  0.0000     0.6984 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617440     3  0.0000     0.6984 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617441     3  0.0000     0.6984 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617443     3  0.0000     0.6984 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617442     3  0.0000     0.6984 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617444     3  0.3672     0.1159 0.368 0.000 0.632 0.000 0.000 0.000
#&gt; SRR1617445     3  0.3672     0.1159 0.368 0.000 0.632 0.000 0.000 0.000
#&gt; SRR1617446     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617447     1  0.3857     0.4508 0.532 0.000 0.468 0.000 0.000 0.000
#&gt; SRR1617448     3  0.3672     0.1159 0.368 0.000 0.632 0.000 0.000 0.000
#&gt; SRR1617449     3  0.3672     0.1159 0.368 0.000 0.632 0.000 0.000 0.000
#&gt; SRR1617451     4  0.5468     1.0000 0.000 0.156 0.000 0.548 0.000 0.296
#&gt; SRR1617450     4  0.5468     1.0000 0.000 0.156 0.000 0.548 0.000 0.296
#&gt; SRR1617452     3  0.0508     0.6945 0.004 0.000 0.984 0.000 0.000 0.012
#&gt; SRR1617454     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617453     3  0.0820     0.6909 0.016 0.000 0.972 0.000 0.000 0.012
#&gt; SRR1617456     6  0.0000     1.0000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617457     6  0.0000     1.0000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617455     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617458     6  0.0000     1.0000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617459     6  0.0000     1.0000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617460     3  0.3847     0.3386 0.000 0.456 0.544 0.000 0.000 0.000
#&gt; SRR1617461     3  0.3847     0.3386 0.000 0.456 0.544 0.000 0.000 0.000
#&gt; SRR1617463     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617462     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.632           0.909       0.941         0.4795 0.508   0.508
#> 3 3 0.514           0.747       0.829         0.2845 0.832   0.670
#> 4 4 0.671           0.821       0.893         0.1060 0.894   0.720
#> 5 5 0.695           0.847       0.871         0.0647 0.925   0.763
#> 6 6 0.695           0.804       0.805         0.0668 0.978   0.910
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617431     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617410     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617411     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617412     2  0.6148      0.880 0.152 0.848
#&gt; SRR1617413     2  0.6148      0.880 0.152 0.848
#&gt; SRR1617414     1  0.0938      0.952 0.988 0.012
#&gt; SRR1617415     1  0.0938      0.952 0.988 0.012
#&gt; SRR1617416     1  0.6887      0.770 0.816 0.184
#&gt; SRR1617417     1  0.6887      0.770 0.816 0.184
#&gt; SRR1617418     2  0.3733      0.922 0.072 0.928
#&gt; SRR1617419     2  0.3733      0.922 0.072 0.928
#&gt; SRR1617420     1  0.6438      0.808 0.836 0.164
#&gt; SRR1617421     1  0.6438      0.808 0.836 0.164
#&gt; SRR1617422     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617423     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617424     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617425     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617427     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617426     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617428     2  0.3733      0.922 0.072 0.928
#&gt; SRR1617429     2  0.3733      0.922 0.072 0.928
#&gt; SRR1617432     1  0.1184      0.951 0.984 0.016
#&gt; SRR1617433     1  0.1184      0.951 0.984 0.016
#&gt; SRR1617434     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617436     2  0.3733      0.922 0.072 0.928
#&gt; SRR1617435     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617437     2  0.3733      0.922 0.072 0.928
#&gt; SRR1617438     2  0.6887      0.855 0.184 0.816
#&gt; SRR1617439     2  0.6887      0.855 0.184 0.816
#&gt; SRR1617440     2  0.5629      0.894 0.132 0.868
#&gt; SRR1617441     2  0.5629      0.894 0.132 0.868
#&gt; SRR1617443     2  0.6887      0.855 0.184 0.816
#&gt; SRR1617442     2  0.6887      0.855 0.184 0.816
#&gt; SRR1617444     2  0.7950      0.786 0.240 0.760
#&gt; SRR1617445     2  0.7950      0.786 0.240 0.760
#&gt; SRR1617446     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617447     1  0.0000      0.956 1.000 0.000
#&gt; SRR1617448     1  0.2043      0.942 0.968 0.032
#&gt; SRR1617449     1  0.2043      0.942 0.968 0.032
#&gt; SRR1617451     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617450     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617452     2  0.3879      0.921 0.076 0.924
#&gt; SRR1617454     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617453     2  0.3879      0.921 0.076 0.924
#&gt; SRR1617456     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617457     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617455     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617458     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617459     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617460     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617461     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617463     2  0.0000      0.919 0.000 1.000
#&gt; SRR1617462     2  0.0000      0.919 0.000 1.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2   0.375      0.775 0.000 0.856 0.144
#&gt; SRR1617431     2   0.375      0.775 0.000 0.856 0.144
#&gt; SRR1617410     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617411     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617412     3   0.271      0.819 0.088 0.000 0.912
#&gt; SRR1617413     3   0.271      0.819 0.088 0.000 0.912
#&gt; SRR1617414     1   0.319      0.807 0.888 0.000 0.112
#&gt; SRR1617415     1   0.319      0.807 0.888 0.000 0.112
#&gt; SRR1617416     1   0.729     -0.305 0.492 0.028 0.480
#&gt; SRR1617417     1   0.729     -0.305 0.492 0.028 0.480
#&gt; SRR1617418     3   0.236      0.812 0.072 0.000 0.928
#&gt; SRR1617419     3   0.236      0.812 0.072 0.000 0.928
#&gt; SRR1617420     1   0.369      0.784 0.860 0.000 0.140
#&gt; SRR1617421     1   0.369      0.784 0.860 0.000 0.140
#&gt; SRR1617422     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617423     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617424     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617425     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617427     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617426     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617428     3   0.377      0.799 0.084 0.028 0.888
#&gt; SRR1617429     3   0.377      0.799 0.084 0.028 0.888
#&gt; SRR1617432     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617433     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617434     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617436     3   0.245      0.814 0.076 0.000 0.924
#&gt; SRR1617435     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617437     3   0.245      0.814 0.076 0.000 0.924
#&gt; SRR1617438     3   0.288      0.821 0.096 0.000 0.904
#&gt; SRR1617439     3   0.288      0.821 0.096 0.000 0.904
#&gt; SRR1617440     3   0.625      0.707 0.268 0.024 0.708
#&gt; SRR1617441     3   0.625      0.707 0.268 0.024 0.708
#&gt; SRR1617443     3   0.288      0.821 0.096 0.000 0.904
#&gt; SRR1617442     3   0.288      0.821 0.096 0.000 0.904
#&gt; SRR1617444     3   0.722      0.439 0.424 0.028 0.548
#&gt; SRR1617445     3   0.722      0.439 0.424 0.028 0.548
#&gt; SRR1617446     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617447     1   0.000      0.887 1.000 0.000 0.000
#&gt; SRR1617448     1   0.375      0.756 0.856 0.000 0.144
#&gt; SRR1617449     1   0.394      0.739 0.844 0.000 0.156
#&gt; SRR1617451     2   0.465      0.886 0.000 0.792 0.208
#&gt; SRR1617450     2   0.465      0.886 0.000 0.792 0.208
#&gt; SRR1617452     3   0.630      0.713 0.260 0.028 0.712
#&gt; SRR1617454     2   0.341      0.899 0.000 0.876 0.124
#&gt; SRR1617453     3   0.630      0.713 0.260 0.028 0.712
#&gt; SRR1617456     2   0.434      0.901 0.016 0.848 0.136
#&gt; SRR1617457     2   0.434      0.901 0.016 0.848 0.136
#&gt; SRR1617455     2   0.341      0.899 0.000 0.876 0.124
#&gt; SRR1617458     2   0.434      0.901 0.016 0.848 0.136
#&gt; SRR1617459     2   0.434      0.901 0.016 0.848 0.136
#&gt; SRR1617460     3   0.661      0.015 0.016 0.356 0.628
#&gt; SRR1617461     3   0.661      0.015 0.016 0.356 0.628
#&gt; SRR1617463     2   0.529      0.794 0.000 0.732 0.268
#&gt; SRR1617462     2   0.529      0.794 0.000 0.732 0.268
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     4  0.4643      1.000 0.000 0.344 0.000 0.656
#&gt; SRR1617431     4  0.4643      1.000 0.000 0.344 0.000 0.656
#&gt; SRR1617410     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617412     3  0.0188      0.877 0.004 0.000 0.996 0.000
#&gt; SRR1617413     3  0.0188      0.877 0.004 0.000 0.996 0.000
#&gt; SRR1617414     1  0.2831      0.822 0.876 0.000 0.120 0.004
#&gt; SRR1617415     1  0.2831      0.822 0.876 0.000 0.120 0.004
#&gt; SRR1617416     3  0.5701      0.683 0.276 0.048 0.672 0.004
#&gt; SRR1617417     3  0.5701      0.683 0.276 0.048 0.672 0.004
#&gt; SRR1617418     3  0.0000      0.875 0.000 0.000 1.000 0.000
#&gt; SRR1617419     3  0.0000      0.875 0.000 0.000 1.000 0.000
#&gt; SRR1617420     1  0.4088      0.709 0.764 0.000 0.232 0.004
#&gt; SRR1617421     1  0.4088      0.709 0.764 0.000 0.232 0.004
#&gt; SRR1617422     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617428     3  0.1576      0.870 0.000 0.048 0.948 0.004
#&gt; SRR1617429     3  0.1576      0.870 0.000 0.048 0.948 0.004
#&gt; SRR1617432     1  0.0188      0.917 0.996 0.000 0.004 0.000
#&gt; SRR1617433     1  0.0188      0.917 0.996 0.000 0.004 0.000
#&gt; SRR1617434     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617436     3  0.0000      0.875 0.000 0.000 1.000 0.000
#&gt; SRR1617435     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617437     3  0.0000      0.875 0.000 0.000 1.000 0.000
#&gt; SRR1617438     3  0.1940      0.868 0.076 0.000 0.924 0.000
#&gt; SRR1617439     3  0.1940      0.868 0.076 0.000 0.924 0.000
#&gt; SRR1617440     3  0.2759      0.875 0.044 0.052 0.904 0.000
#&gt; SRR1617441     3  0.2759      0.875 0.044 0.052 0.904 0.000
#&gt; SRR1617443     3  0.1940      0.868 0.076 0.000 0.924 0.000
#&gt; SRR1617442     3  0.1940      0.868 0.076 0.000 0.924 0.000
#&gt; SRR1617444     3  0.5669      0.701 0.260 0.052 0.684 0.004
#&gt; SRR1617445     3  0.5669      0.701 0.260 0.052 0.684 0.004
#&gt; SRR1617446     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.3982      0.685 0.776 0.000 0.220 0.004
#&gt; SRR1617449     1  0.4018      0.678 0.772 0.000 0.224 0.004
#&gt; SRR1617451     2  0.3024      0.719 0.000 0.852 0.148 0.000
#&gt; SRR1617450     2  0.3024      0.719 0.000 0.852 0.148 0.000
#&gt; SRR1617452     3  0.1474      0.869 0.000 0.052 0.948 0.000
#&gt; SRR1617454     2  0.0000      0.705 0.000 1.000 0.000 0.000
#&gt; SRR1617453     3  0.1474      0.869 0.000 0.052 0.948 0.000
#&gt; SRR1617456     2  0.4936      0.675 0.000 0.652 0.008 0.340
#&gt; SRR1617457     2  0.4936      0.675 0.000 0.652 0.008 0.340
#&gt; SRR1617455     2  0.0000      0.705 0.000 1.000 0.000 0.000
#&gt; SRR1617458     2  0.4936      0.675 0.000 0.652 0.008 0.340
#&gt; SRR1617459     2  0.4936      0.675 0.000 0.652 0.008 0.340
#&gt; SRR1617460     2  0.3569      0.672 0.000 0.804 0.196 0.000
#&gt; SRR1617461     2  0.3486      0.683 0.000 0.812 0.188 0.000
#&gt; SRR1617463     2  0.0000      0.705 0.000 1.000 0.000 0.000
#&gt; SRR1617462     2  0.0000      0.705 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     4  0.1121      1.000 0.000 0.044 0.000 0.956 0.000
#&gt; SRR1617431     4  0.1121      1.000 0.000 0.044 0.000 0.956 0.000
#&gt; SRR1617410     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617412     3  0.0000      0.933 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617413     3  0.0000      0.933 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617414     1  0.3209      0.757 0.848 0.000 0.120 0.028 0.004
#&gt; SRR1617415     1  0.3209      0.757 0.848 0.000 0.120 0.028 0.004
#&gt; SRR1617416     5  0.6120      0.904 0.256 0.000 0.184 0.000 0.560
#&gt; SRR1617417     5  0.6120      0.904 0.256 0.000 0.184 0.000 0.560
#&gt; SRR1617418     3  0.0771      0.933 0.000 0.000 0.976 0.004 0.020
#&gt; SRR1617419     3  0.0771      0.933 0.000 0.000 0.976 0.004 0.020
#&gt; SRR1617420     1  0.4121      0.557 0.720 0.000 0.264 0.012 0.004
#&gt; SRR1617421     1  0.4121      0.557 0.720 0.000 0.264 0.012 0.004
#&gt; SRR1617422     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617428     3  0.1026      0.932 0.004 0.000 0.968 0.004 0.024
#&gt; SRR1617429     3  0.1026      0.932 0.004 0.000 0.968 0.004 0.024
#&gt; SRR1617432     1  0.0162      0.917 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1617433     1  0.0162      0.917 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1617434     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617436     3  0.0771      0.933 0.000 0.000 0.976 0.004 0.020
#&gt; SRR1617435     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617437     3  0.0771      0.933 0.000 0.000 0.976 0.004 0.020
#&gt; SRR1617438     3  0.2325      0.873 0.068 0.000 0.904 0.028 0.000
#&gt; SRR1617439     3  0.2325      0.873 0.068 0.000 0.904 0.028 0.000
#&gt; SRR1617440     3  0.1412      0.924 0.008 0.004 0.952 0.036 0.000
#&gt; SRR1617441     3  0.1412      0.924 0.008 0.004 0.952 0.036 0.000
#&gt; SRR1617443     3  0.2491      0.870 0.068 0.000 0.896 0.036 0.000
#&gt; SRR1617442     3  0.2491      0.870 0.068 0.000 0.896 0.036 0.000
#&gt; SRR1617444     5  0.6272      0.867 0.200 0.000 0.236 0.004 0.560
#&gt; SRR1617445     5  0.6272      0.867 0.200 0.000 0.236 0.004 0.560
#&gt; SRR1617446     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.920 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617448     5  0.6121      0.870 0.324 0.000 0.148 0.000 0.528
#&gt; SRR1617449     5  0.6121      0.870 0.324 0.000 0.148 0.000 0.528
#&gt; SRR1617451     2  0.2886      0.725 0.000 0.844 0.148 0.000 0.008
#&gt; SRR1617450     2  0.2886      0.725 0.000 0.844 0.148 0.000 0.008
#&gt; SRR1617452     3  0.1697      0.894 0.000 0.060 0.932 0.008 0.000
#&gt; SRR1617454     2  0.0000      0.735 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617453     3  0.1697      0.894 0.000 0.060 0.932 0.008 0.000
#&gt; SRR1617456     2  0.4383      0.657 0.000 0.572 0.000 0.004 0.424
#&gt; SRR1617457     2  0.4383      0.657 0.000 0.572 0.000 0.004 0.424
#&gt; SRR1617455     2  0.0000      0.735 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617458     2  0.4383      0.657 0.000 0.572 0.000 0.004 0.424
#&gt; SRR1617459     2  0.4383      0.657 0.000 0.572 0.000 0.004 0.424
#&gt; SRR1617460     2  0.4035      0.670 0.000 0.756 0.220 0.008 0.016
#&gt; SRR1617461     2  0.4035      0.670 0.000 0.756 0.220 0.008 0.016
#&gt; SRR1617463     2  0.0000      0.735 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617462     2  0.0000      0.735 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     4  0.5916      1.000 0.336 0.000 0.000 0.444 0.000 0.220
#&gt; SRR1617431     4  0.5916      1.000 0.336 0.000 0.000 0.444 0.000 0.220
#&gt; SRR1617410     5  0.0291      0.875 0.004 0.000 0.004 0.000 0.992 0.000
#&gt; SRR1617411     5  0.0291      0.875 0.004 0.000 0.004 0.000 0.992 0.000
#&gt; SRR1617412     3  0.2020      0.734 0.008 0.000 0.896 0.096 0.000 0.000
#&gt; SRR1617413     3  0.2020      0.734 0.008 0.000 0.896 0.096 0.000 0.000
#&gt; SRR1617414     5  0.3789      0.721 0.040 0.000 0.160 0.016 0.784 0.000
#&gt; SRR1617415     5  0.3789      0.721 0.040 0.000 0.160 0.016 0.784 0.000
#&gt; SRR1617416     1  0.4632      0.875 0.656 0.000 0.064 0.004 0.276 0.000
#&gt; SRR1617417     1  0.4598      0.874 0.656 0.000 0.060 0.004 0.280 0.000
#&gt; SRR1617418     3  0.4062      0.624 0.008 0.000 0.552 0.440 0.000 0.000
#&gt; SRR1617419     3  0.4062      0.624 0.008 0.000 0.552 0.440 0.000 0.000
#&gt; SRR1617420     5  0.4023      0.677 0.016 0.000 0.124 0.080 0.780 0.000
#&gt; SRR1617421     5  0.4023      0.677 0.016 0.000 0.124 0.080 0.780 0.000
#&gt; SRR1617422     5  0.0777      0.872 0.024 0.000 0.004 0.000 0.972 0.000
#&gt; SRR1617423     5  0.0777      0.872 0.024 0.000 0.004 0.000 0.972 0.000
#&gt; SRR1617424     5  0.1757      0.844 0.076 0.000 0.008 0.000 0.916 0.000
#&gt; SRR1617425     5  0.1757      0.844 0.076 0.000 0.008 0.000 0.916 0.000
#&gt; SRR1617427     5  0.1349      0.860 0.056 0.000 0.004 0.000 0.940 0.000
#&gt; SRR1617426     5  0.1349      0.860 0.056 0.000 0.004 0.000 0.940 0.000
#&gt; SRR1617428     3  0.5464      0.559 0.016 0.000 0.456 0.452 0.076 0.000
#&gt; SRR1617429     3  0.5464      0.559 0.016 0.000 0.456 0.452 0.076 0.000
#&gt; SRR1617432     5  0.1434      0.865 0.020 0.000 0.024 0.008 0.948 0.000
#&gt; SRR1617433     5  0.1434      0.865 0.020 0.000 0.024 0.008 0.948 0.000
#&gt; SRR1617434     5  0.0146      0.875 0.000 0.000 0.004 0.000 0.996 0.000
#&gt; SRR1617436     3  0.4083      0.617 0.008 0.000 0.532 0.460 0.000 0.000
#&gt; SRR1617435     5  0.0146      0.875 0.000 0.000 0.004 0.000 0.996 0.000
#&gt; SRR1617437     3  0.4083      0.617 0.008 0.000 0.532 0.460 0.000 0.000
#&gt; SRR1617438     3  0.1605      0.714 0.004 0.000 0.936 0.000 0.044 0.016
#&gt; SRR1617439     3  0.1605      0.714 0.004 0.000 0.936 0.000 0.044 0.016
#&gt; SRR1617440     3  0.2925      0.685 0.016 0.000 0.880 0.024 0.048 0.032
#&gt; SRR1617441     3  0.2925      0.685 0.016 0.000 0.880 0.024 0.048 0.032
#&gt; SRR1617443     3  0.2706      0.699 0.048 0.016 0.888 0.008 0.040 0.000
#&gt; SRR1617442     3  0.2706      0.699 0.048 0.016 0.888 0.008 0.040 0.000
#&gt; SRR1617444     1  0.5055      0.790 0.652 0.004 0.184 0.000 0.160 0.000
#&gt; SRR1617445     1  0.5055      0.790 0.652 0.004 0.184 0.000 0.160 0.000
#&gt; SRR1617446     5  0.1843      0.846 0.080 0.000 0.004 0.004 0.912 0.000
#&gt; SRR1617447     5  0.1956      0.848 0.080 0.000 0.004 0.008 0.908 0.000
#&gt; SRR1617448     1  0.4660      0.844 0.612 0.000 0.060 0.000 0.328 0.000
#&gt; SRR1617449     1  0.4660      0.844 0.612 0.000 0.060 0.000 0.328 0.000
#&gt; SRR1617451     2  0.3061      0.860 0.008 0.840 0.020 0.128 0.000 0.004
#&gt; SRR1617450     2  0.3061      0.860 0.008 0.840 0.020 0.128 0.000 0.004
#&gt; SRR1617452     3  0.2720      0.729 0.004 0.016 0.884 0.056 0.000 0.040
#&gt; SRR1617454     2  0.0000      0.868 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617453     3  0.2720      0.729 0.004 0.016 0.884 0.056 0.000 0.040
#&gt; SRR1617456     6  0.3449      0.995 0.008 0.196 0.016 0.000 0.000 0.780
#&gt; SRR1617457     6  0.3449      0.995 0.008 0.196 0.016 0.000 0.000 0.780
#&gt; SRR1617455     2  0.0000      0.868 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617458     6  0.3457      0.995 0.012 0.196 0.012 0.000 0.000 0.780
#&gt; SRR1617459     6  0.3457      0.995 0.012 0.196 0.012 0.000 0.000 0.780
#&gt; SRR1617460     2  0.4529      0.823 0.020 0.776 0.068 0.096 0.000 0.040
#&gt; SRR1617461     2  0.4529      0.823 0.020 0.776 0.068 0.096 0.000 0.040
#&gt; SRR1617463     2  0.0993      0.864 0.000 0.964 0.000 0.012 0.000 0.024
#&gt; SRR1617462     2  0.0993      0.864 0.000 0.964 0.000 0.012 0.000 0.024
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.852           0.931       0.971         0.4357 0.547   0.547
#> 3 3 1.000           0.957       0.981         0.5071 0.726   0.528
#> 4 4 0.799           0.744       0.823         0.0899 0.962   0.887
#> 5 5 0.749           0.792       0.786         0.0919 0.867   0.575
#> 6 6 0.893           0.750       0.854         0.0575 0.952   0.766
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2   0.295      0.891 0.052 0.948
#&gt; SRR1617431     2   0.204      0.903 0.032 0.968
#&gt; SRR1617410     1   0.000      0.993 1.000 0.000
#&gt; SRR1617411     1   0.000      0.993 1.000 0.000
#&gt; SRR1617412     1   0.000      0.993 1.000 0.000
#&gt; SRR1617413     1   0.000      0.993 1.000 0.000
#&gt; SRR1617414     1   0.000      0.993 1.000 0.000
#&gt; SRR1617415     1   0.000      0.993 1.000 0.000
#&gt; SRR1617416     1   0.000      0.993 1.000 0.000
#&gt; SRR1617417     1   0.000      0.993 1.000 0.000
#&gt; SRR1617418     1   0.000      0.993 1.000 0.000
#&gt; SRR1617419     1   0.000      0.993 1.000 0.000
#&gt; SRR1617420     1   0.000      0.993 1.000 0.000
#&gt; SRR1617421     1   0.000      0.993 1.000 0.000
#&gt; SRR1617422     1   0.000      0.993 1.000 0.000
#&gt; SRR1617423     1   0.000      0.993 1.000 0.000
#&gt; SRR1617424     1   0.000      0.993 1.000 0.000
#&gt; SRR1617425     1   0.000      0.993 1.000 0.000
#&gt; SRR1617427     1   0.000      0.993 1.000 0.000
#&gt; SRR1617426     1   0.000      0.993 1.000 0.000
#&gt; SRR1617428     1   0.469      0.878 0.900 0.100
#&gt; SRR1617429     1   0.563      0.833 0.868 0.132
#&gt; SRR1617432     1   0.000      0.993 1.000 0.000
#&gt; SRR1617433     1   0.000      0.993 1.000 0.000
#&gt; SRR1617434     1   0.000      0.993 1.000 0.000
#&gt; SRR1617436     1   0.000      0.993 1.000 0.000
#&gt; SRR1617435     1   0.000      0.993 1.000 0.000
#&gt; SRR1617437     1   0.000      0.993 1.000 0.000
#&gt; SRR1617438     1   0.000      0.993 1.000 0.000
#&gt; SRR1617439     1   0.000      0.993 1.000 0.000
#&gt; SRR1617440     2   0.998      0.190 0.476 0.524
#&gt; SRR1617441     2   0.994      0.252 0.456 0.544
#&gt; SRR1617443     1   0.000      0.993 1.000 0.000
#&gt; SRR1617442     1   0.000      0.993 1.000 0.000
#&gt; SRR1617444     1   0.000      0.993 1.000 0.000
#&gt; SRR1617445     1   0.000      0.993 1.000 0.000
#&gt; SRR1617446     1   0.000      0.993 1.000 0.000
#&gt; SRR1617447     1   0.000      0.993 1.000 0.000
#&gt; SRR1617448     1   0.000      0.993 1.000 0.000
#&gt; SRR1617449     1   0.000      0.993 1.000 0.000
#&gt; SRR1617451     2   0.000      0.918 0.000 1.000
#&gt; SRR1617450     2   0.000      0.918 0.000 1.000
#&gt; SRR1617452     2   0.662      0.788 0.172 0.828
#&gt; SRR1617454     2   0.000      0.918 0.000 1.000
#&gt; SRR1617453     2   0.662      0.788 0.172 0.828
#&gt; SRR1617456     2   0.000      0.918 0.000 1.000
#&gt; SRR1617457     2   0.000      0.918 0.000 1.000
#&gt; SRR1617455     2   0.000      0.918 0.000 1.000
#&gt; SRR1617458     2   0.000      0.918 0.000 1.000
#&gt; SRR1617459     2   0.000      0.918 0.000 1.000
#&gt; SRR1617460     2   0.000      0.918 0.000 1.000
#&gt; SRR1617461     2   0.000      0.918 0.000 1.000
#&gt; SRR1617463     2   0.000      0.918 0.000 1.000
#&gt; SRR1617462     2   0.000      0.918 0.000 1.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     3  0.0000      0.989 0.000 0.000 1.000
#&gt; SRR1617431     3  0.0000      0.989 0.000 0.000 1.000
#&gt; SRR1617410     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617412     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617413     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617414     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617415     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617416     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617417     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617418     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617419     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617420     1  0.0424      0.992 0.992 0.000 0.008
#&gt; SRR1617421     1  0.0424      0.992 0.992 0.000 0.008
#&gt; SRR1617422     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617428     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617429     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617432     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617433     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617434     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617436     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617435     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617437     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617438     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617439     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617440     3  0.0592      0.986 0.000 0.012 0.988
#&gt; SRR1617441     3  0.1289      0.967 0.000 0.032 0.968
#&gt; SRR1617443     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617442     3  0.0424      0.995 0.008 0.000 0.992
#&gt; SRR1617444     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617445     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617446     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.999 1.000 0.000 0.000
#&gt; SRR1617451     2  0.0424      0.931 0.000 0.992 0.008
#&gt; SRR1617450     2  0.0424      0.931 0.000 0.992 0.008
#&gt; SRR1617452     2  0.6168      0.343 0.000 0.588 0.412
#&gt; SRR1617454     2  0.0424      0.931 0.000 0.992 0.008
#&gt; SRR1617453     2  0.6180      0.333 0.000 0.584 0.416
#&gt; SRR1617456     2  0.0000      0.931 0.000 1.000 0.000
#&gt; SRR1617457     2  0.0000      0.931 0.000 1.000 0.000
#&gt; SRR1617455     2  0.0424      0.931 0.000 0.992 0.008
#&gt; SRR1617458     2  0.0000      0.931 0.000 1.000 0.000
#&gt; SRR1617459     2  0.0000      0.931 0.000 1.000 0.000
#&gt; SRR1617460     2  0.0000      0.931 0.000 1.000 0.000
#&gt; SRR1617461     2  0.0000      0.931 0.000 1.000 0.000
#&gt; SRR1617463     2  0.0424      0.931 0.000 0.992 0.008
#&gt; SRR1617462     2  0.0424      0.931 0.000 0.992 0.008
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     4  0.4624     0.8201 0.000 0.000 0.340 0.660
#&gt; SRR1617431     4  0.4624     0.8201 0.000 0.000 0.340 0.660
#&gt; SRR1617410     1  0.0000     0.8772 1.000 0.000 0.000 0.000
#&gt; SRR1617411     1  0.0000     0.8772 1.000 0.000 0.000 0.000
#&gt; SRR1617412     3  0.1302     0.7323 0.000 0.000 0.956 0.044
#&gt; SRR1617413     3  0.1302     0.7323 0.000 0.000 0.956 0.044
#&gt; SRR1617414     1  0.0000     0.8772 1.000 0.000 0.000 0.000
#&gt; SRR1617415     1  0.0000     0.8772 1.000 0.000 0.000 0.000
#&gt; SRR1617416     1  0.4426     0.9039 0.772 0.000 0.024 0.204
#&gt; SRR1617417     1  0.4361     0.9041 0.772 0.000 0.020 0.208
#&gt; SRR1617418     4  0.4999     0.6904 0.000 0.000 0.492 0.508
#&gt; SRR1617419     3  0.4999    -0.7388 0.000 0.000 0.508 0.492
#&gt; SRR1617420     1  0.0376     0.8722 0.992 0.000 0.004 0.004
#&gt; SRR1617421     1  0.0376     0.8722 0.992 0.000 0.004 0.004
#&gt; SRR1617422     1  0.3791     0.9088 0.796 0.000 0.004 0.200
#&gt; SRR1617423     1  0.3933     0.9087 0.792 0.000 0.008 0.200
#&gt; SRR1617424     1  0.4175     0.9078 0.784 0.000 0.016 0.200
#&gt; SRR1617425     1  0.4175     0.9078 0.784 0.000 0.016 0.200
#&gt; SRR1617427     1  0.3610     0.9086 0.800 0.000 0.000 0.200
#&gt; SRR1617426     1  0.3610     0.9086 0.800 0.000 0.000 0.200
#&gt; SRR1617428     4  0.4907     0.8484 0.000 0.000 0.420 0.580
#&gt; SRR1617429     4  0.4907     0.8484 0.000 0.000 0.420 0.580
#&gt; SRR1617432     1  0.0000     0.8772 1.000 0.000 0.000 0.000
#&gt; SRR1617433     1  0.0000     0.8772 1.000 0.000 0.000 0.000
#&gt; SRR1617434     1  0.0000     0.8772 1.000 0.000 0.000 0.000
#&gt; SRR1617436     3  0.4277     0.1746 0.000 0.000 0.720 0.280
#&gt; SRR1617435     1  0.0000     0.8772 1.000 0.000 0.000 0.000
#&gt; SRR1617437     3  0.4431     0.0564 0.000 0.000 0.696 0.304
#&gt; SRR1617438     3  0.0000     0.7556 0.000 0.000 1.000 0.000
#&gt; SRR1617439     3  0.0000     0.7556 0.000 0.000 1.000 0.000
#&gt; SRR1617440     3  0.2124     0.6886 0.000 0.068 0.924 0.008
#&gt; SRR1617441     3  0.2048     0.6934 0.000 0.064 0.928 0.008
#&gt; SRR1617443     3  0.0000     0.7556 0.000 0.000 1.000 0.000
#&gt; SRR1617442     3  0.0000     0.7556 0.000 0.000 1.000 0.000
#&gt; SRR1617444     1  0.4793     0.8947 0.756 0.000 0.040 0.204
#&gt; SRR1617445     1  0.4793     0.8947 0.756 0.000 0.040 0.204
#&gt; SRR1617446     1  0.4175     0.9078 0.784 0.000 0.016 0.200
#&gt; SRR1617447     1  0.4175     0.9078 0.784 0.000 0.016 0.200
#&gt; SRR1617448     1  0.4214     0.9068 0.780 0.000 0.016 0.204
#&gt; SRR1617449     1  0.4214     0.9068 0.780 0.000 0.016 0.204
#&gt; SRR1617451     2  0.4356     0.7671 0.000 0.708 0.000 0.292
#&gt; SRR1617450     2  0.4382     0.7648 0.000 0.704 0.000 0.296
#&gt; SRR1617452     2  0.7937    -0.0812 0.008 0.452 0.236 0.304
#&gt; SRR1617454     2  0.4103     0.7837 0.000 0.744 0.000 0.256
#&gt; SRR1617453     2  0.7937    -0.0812 0.008 0.452 0.236 0.304
#&gt; SRR1617456     2  0.0524     0.8046 0.000 0.988 0.004 0.008
#&gt; SRR1617457     2  0.0524     0.8046 0.000 0.988 0.004 0.008
#&gt; SRR1617455     2  0.4103     0.7837 0.000 0.744 0.000 0.256
#&gt; SRR1617458     2  0.0524     0.8046 0.000 0.988 0.004 0.008
#&gt; SRR1617459     2  0.0524     0.8046 0.000 0.988 0.004 0.008
#&gt; SRR1617460     2  0.0336     0.8055 0.000 0.992 0.000 0.008
#&gt; SRR1617461     2  0.0336     0.8055 0.000 0.992 0.000 0.008
#&gt; SRR1617463     2  0.3764     0.7953 0.000 0.784 0.000 0.216
#&gt; SRR1617462     2  0.3764     0.7953 0.000 0.784 0.000 0.216
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     4  0.2605      0.699 0.000 0.000 0.148 0.852 0.000
#&gt; SRR1617431     4  0.2605      0.699 0.000 0.000 0.148 0.852 0.000
#&gt; SRR1617410     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617411     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617412     3  0.1478      0.808 0.000 0.000 0.936 0.064 0.000
#&gt; SRR1617413     3  0.1478      0.808 0.000 0.000 0.936 0.064 0.000
#&gt; SRR1617414     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617415     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617416     1  0.5379      0.852 0.676 0.000 0.016 0.076 0.232
#&gt; SRR1617417     1  0.5379      0.852 0.676 0.000 0.016 0.076 0.232
#&gt; SRR1617418     4  0.4126      0.494 0.000 0.000 0.380 0.620 0.000
#&gt; SRR1617419     4  0.4227      0.415 0.000 0.000 0.420 0.580 0.000
#&gt; SRR1617420     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617421     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617422     1  0.3966      0.961 0.664 0.000 0.000 0.000 0.336
#&gt; SRR1617423     1  0.3966      0.961 0.664 0.000 0.000 0.000 0.336
#&gt; SRR1617424     1  0.3949      0.963 0.668 0.000 0.000 0.000 0.332
#&gt; SRR1617425     1  0.3949      0.963 0.668 0.000 0.000 0.000 0.332
#&gt; SRR1617427     1  0.3966      0.961 0.664 0.000 0.000 0.000 0.336
#&gt; SRR1617426     1  0.3966      0.961 0.664 0.000 0.000 0.000 0.336
#&gt; SRR1617428     4  0.3003      0.700 0.000 0.000 0.188 0.812 0.000
#&gt; SRR1617429     4  0.3003      0.700 0.000 0.000 0.188 0.812 0.000
#&gt; SRR1617432     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617433     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617434     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617436     3  0.4166      0.325 0.004 0.000 0.648 0.348 0.000
#&gt; SRR1617435     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617437     3  0.4182      0.313 0.004 0.000 0.644 0.352 0.000
#&gt; SRR1617438     3  0.0290      0.836 0.008 0.000 0.992 0.000 0.000
#&gt; SRR1617439     3  0.0290      0.836 0.008 0.000 0.992 0.000 0.000
#&gt; SRR1617440     3  0.2387      0.776 0.040 0.048 0.908 0.004 0.000
#&gt; SRR1617441     3  0.2609      0.765 0.052 0.048 0.896 0.004 0.000
#&gt; SRR1617443     3  0.0290      0.836 0.008 0.000 0.992 0.000 0.000
#&gt; SRR1617442     3  0.0290      0.836 0.008 0.000 0.992 0.000 0.000
#&gt; SRR1617444     1  0.4492      0.933 0.680 0.020 0.004 0.000 0.296
#&gt; SRR1617445     1  0.4492      0.933 0.680 0.020 0.004 0.000 0.296
#&gt; SRR1617446     1  0.3949      0.963 0.668 0.000 0.000 0.000 0.332
#&gt; SRR1617447     1  0.3949      0.963 0.668 0.000 0.000 0.000 0.332
#&gt; SRR1617448     1  0.3949      0.963 0.668 0.000 0.000 0.000 0.332
#&gt; SRR1617449     1  0.3949      0.963 0.668 0.000 0.000 0.000 0.332
#&gt; SRR1617451     2  0.6517      0.573 0.192 0.416 0.000 0.392 0.000
#&gt; SRR1617450     2  0.6519      0.562 0.192 0.408 0.000 0.400 0.000
#&gt; SRR1617452     4  0.8123      0.323 0.128 0.332 0.124 0.404 0.012
#&gt; SRR1617454     2  0.5618      0.683 0.136 0.628 0.000 0.236 0.000
#&gt; SRR1617453     4  0.8112      0.341 0.128 0.324 0.124 0.412 0.012
#&gt; SRR1617456     2  0.3289      0.682 0.172 0.816 0.008 0.004 0.000
#&gt; SRR1617457     2  0.3289      0.682 0.172 0.816 0.008 0.004 0.000
#&gt; SRR1617455     2  0.5618      0.683 0.136 0.628 0.000 0.236 0.000
#&gt; SRR1617458     2  0.3289      0.682 0.172 0.816 0.008 0.004 0.000
#&gt; SRR1617459     2  0.3289      0.682 0.172 0.816 0.008 0.004 0.000
#&gt; SRR1617460     2  0.0404      0.707 0.012 0.988 0.000 0.000 0.000
#&gt; SRR1617461     2  0.0404      0.707 0.012 0.988 0.000 0.000 0.000
#&gt; SRR1617463     2  0.5264      0.702 0.128 0.676 0.000 0.196 0.000
#&gt; SRR1617462     2  0.5264      0.702 0.128 0.676 0.000 0.196 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     4  0.1059      0.574 0.000 0.016 0.016 0.964 0.004 0.000
#&gt; SRR1617431     4  0.1059      0.574 0.000 0.016 0.016 0.964 0.004 0.000
#&gt; SRR1617410     5  0.1387      0.996 0.068 0.000 0.000 0.000 0.932 0.000
#&gt; SRR1617411     5  0.1387      0.996 0.068 0.000 0.000 0.000 0.932 0.000
#&gt; SRR1617412     3  0.3739      0.637 0.004 0.020 0.772 0.192 0.012 0.000
#&gt; SRR1617413     3  0.3739      0.637 0.004 0.020 0.772 0.192 0.012 0.000
#&gt; SRR1617414     5  0.1787      0.993 0.068 0.008 0.000 0.004 0.920 0.000
#&gt; SRR1617415     5  0.1787      0.993 0.068 0.008 0.000 0.004 0.920 0.000
#&gt; SRR1617416     1  0.1837      0.921 0.932 0.032 0.004 0.012 0.020 0.000
#&gt; SRR1617417     1  0.1837      0.921 0.932 0.032 0.004 0.012 0.020 0.000
#&gt; SRR1617418     4  0.4302      0.307 0.004 0.012 0.344 0.632 0.008 0.000
#&gt; SRR1617419     4  0.4398      0.244 0.004 0.012 0.376 0.600 0.008 0.000
#&gt; SRR1617420     5  0.1387      0.996 0.068 0.000 0.000 0.000 0.932 0.000
#&gt; SRR1617421     5  0.1387      0.996 0.068 0.000 0.000 0.000 0.932 0.000
#&gt; SRR1617422     1  0.0363      0.984 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; SRR1617423     1  0.0363      0.984 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; SRR1617424     1  0.0363      0.984 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; SRR1617425     1  0.0363      0.984 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; SRR1617427     1  0.0458      0.981 0.984 0.000 0.000 0.000 0.016 0.000
#&gt; SRR1617426     1  0.0458      0.981 0.984 0.000 0.000 0.000 0.016 0.000
#&gt; SRR1617428     4  0.2837      0.595 0.008 0.060 0.032 0.880 0.020 0.000
#&gt; SRR1617429     4  0.2837      0.595 0.008 0.060 0.032 0.880 0.020 0.000
#&gt; SRR1617432     5  0.1787      0.993 0.068 0.008 0.000 0.004 0.920 0.000
#&gt; SRR1617433     5  0.1787      0.993 0.068 0.008 0.000 0.004 0.920 0.000
#&gt; SRR1617434     5  0.1387      0.996 0.068 0.000 0.000 0.000 0.932 0.000
#&gt; SRR1617436     4  0.6188      0.299 0.012 0.088 0.396 0.468 0.036 0.000
#&gt; SRR1617435     5  0.1387      0.996 0.068 0.000 0.000 0.000 0.932 0.000
#&gt; SRR1617437     4  0.6184      0.306 0.012 0.088 0.392 0.472 0.036 0.000
#&gt; SRR1617438     3  0.0146      0.839 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1617439     3  0.0146      0.839 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1617440     3  0.2631      0.725 0.000 0.000 0.820 0.000 0.000 0.180
#&gt; SRR1617441     3  0.2631      0.725 0.000 0.000 0.820 0.000 0.000 0.180
#&gt; SRR1617443     3  0.0146      0.839 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1617442     3  0.0146      0.839 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1617444     1  0.0405      0.978 0.988 0.000 0.008 0.000 0.000 0.004
#&gt; SRR1617445     1  0.0405      0.978 0.988 0.000 0.008 0.000 0.000 0.004
#&gt; SRR1617446     1  0.0363      0.984 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; SRR1617447     1  0.0363      0.984 0.988 0.000 0.000 0.000 0.012 0.000
#&gt; SRR1617448     1  0.0405      0.983 0.988 0.000 0.004 0.000 0.008 0.000
#&gt; SRR1617449     1  0.0405      0.983 0.988 0.000 0.004 0.000 0.008 0.000
#&gt; SRR1617451     2  0.5702      0.386 0.000 0.480 0.000 0.384 0.008 0.128
#&gt; SRR1617450     2  0.5677      0.383 0.000 0.480 0.000 0.388 0.008 0.124
#&gt; SRR1617452     4  0.7886      0.154 0.008 0.212 0.072 0.376 0.040 0.292
#&gt; SRR1617454     2  0.2048      0.678 0.000 0.880 0.000 0.000 0.000 0.120
#&gt; SRR1617453     4  0.7886      0.154 0.008 0.212 0.072 0.376 0.040 0.292
#&gt; SRR1617456     6  0.0146      0.760 0.000 0.000 0.004 0.000 0.000 0.996
#&gt; SRR1617457     6  0.0146      0.760 0.000 0.000 0.004 0.000 0.000 0.996
#&gt; SRR1617455     2  0.2048      0.678 0.000 0.880 0.000 0.000 0.000 0.120
#&gt; SRR1617458     6  0.0146      0.760 0.000 0.000 0.004 0.000 0.000 0.996
#&gt; SRR1617459     6  0.0146      0.760 0.000 0.000 0.004 0.000 0.000 0.996
#&gt; SRR1617460     6  0.4566      0.276 0.000 0.452 0.000 0.012 0.016 0.520
#&gt; SRR1617461     6  0.4566      0.276 0.000 0.452 0.000 0.012 0.016 0.520
#&gt; SRR1617463     2  0.2473      0.661 0.000 0.856 0.000 0.000 0.008 0.136
#&gt; SRR1617462     2  0.2473      0.661 0.000 0.856 0.000 0.000 0.008 0.136
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.852           0.947       0.961         0.4819 0.497   0.497
#> 3 3 0.882           0.932       0.970         0.1624 0.925   0.848
#> 4 4 0.913           0.934       0.970         0.2629 0.860   0.668
#> 5 5 0.882           0.861       0.900         0.0473 0.994   0.980
#> 6 6 0.946           0.940       0.967         0.0460 0.944   0.797
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 4
```

There is also optional best $k$ = 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2   0.278      0.930 0.048 0.952
#&gt; SRR1617431     2   0.278      0.930 0.048 0.952
#&gt; SRR1617410     1   0.000      1.000 1.000 0.000
#&gt; SRR1617411     1   0.000      1.000 1.000 0.000
#&gt; SRR1617412     1   0.000      1.000 1.000 0.000
#&gt; SRR1617413     1   0.000      1.000 1.000 0.000
#&gt; SRR1617414     1   0.000      1.000 1.000 0.000
#&gt; SRR1617415     1   0.000      1.000 1.000 0.000
#&gt; SRR1617416     2   0.541      0.855 0.124 0.876
#&gt; SRR1617417     2   0.541      0.855 0.124 0.876
#&gt; SRR1617418     1   0.000      1.000 1.000 0.000
#&gt; SRR1617419     1   0.000      1.000 1.000 0.000
#&gt; SRR1617420     1   0.000      1.000 1.000 0.000
#&gt; SRR1617421     1   0.000      1.000 1.000 0.000
#&gt; SRR1617422     1   0.000      1.000 1.000 0.000
#&gt; SRR1617423     1   0.000      1.000 1.000 0.000
#&gt; SRR1617424     1   0.000      1.000 1.000 0.000
#&gt; SRR1617425     1   0.000      1.000 1.000 0.000
#&gt; SRR1617427     1   0.000      1.000 1.000 0.000
#&gt; SRR1617426     1   0.000      1.000 1.000 0.000
#&gt; SRR1617428     2   0.909      0.627 0.324 0.676
#&gt; SRR1617429     2   0.909      0.627 0.324 0.676
#&gt; SRR1617432     1   0.000      1.000 1.000 0.000
#&gt; SRR1617433     1   0.000      1.000 1.000 0.000
#&gt; SRR1617434     1   0.000      1.000 1.000 0.000
#&gt; SRR1617436     1   0.000      1.000 1.000 0.000
#&gt; SRR1617435     1   0.000      1.000 1.000 0.000
#&gt; SRR1617437     1   0.000      1.000 1.000 0.000
#&gt; SRR1617438     1   0.000      1.000 1.000 0.000
#&gt; SRR1617439     1   0.000      1.000 1.000 0.000
#&gt; SRR1617440     2   0.388      0.916 0.076 0.924
#&gt; SRR1617441     2   0.388      0.916 0.076 0.924
#&gt; SRR1617443     1   0.000      1.000 1.000 0.000
#&gt; SRR1617442     1   0.000      1.000 1.000 0.000
#&gt; SRR1617444     2   0.833      0.722 0.264 0.736
#&gt; SRR1617445     2   0.833      0.722 0.264 0.736
#&gt; SRR1617446     1   0.000      1.000 1.000 0.000
#&gt; SRR1617447     1   0.000      1.000 1.000 0.000
#&gt; SRR1617448     1   0.000      1.000 1.000 0.000
#&gt; SRR1617449     1   0.000      1.000 1.000 0.000
#&gt; SRR1617451     2   0.224      0.933 0.036 0.964
#&gt; SRR1617450     2   0.224      0.933 0.036 0.964
#&gt; SRR1617452     2   0.000      0.909 0.000 1.000
#&gt; SRR1617454     2   0.224      0.933 0.036 0.964
#&gt; SRR1617453     2   0.000      0.909 0.000 1.000
#&gt; SRR1617456     2   0.224      0.933 0.036 0.964
#&gt; SRR1617457     2   0.224      0.933 0.036 0.964
#&gt; SRR1617455     2   0.224      0.933 0.036 0.964
#&gt; SRR1617458     2   0.224      0.933 0.036 0.964
#&gt; SRR1617459     2   0.224      0.933 0.036 0.964
#&gt; SRR1617460     2   0.224      0.933 0.036 0.964
#&gt; SRR1617461     2   0.224      0.933 0.036 0.964
#&gt; SRR1617463     2   0.224      0.933 0.036 0.964
#&gt; SRR1617462     2   0.224      0.933 0.036 0.964
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2   p3
#&gt; SRR1617430     2  0.0592      0.937 0.012 0.988 0.00
#&gt; SRR1617431     2  0.0592      0.937 0.012 0.988 0.00
#&gt; SRR1617410     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617411     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617412     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617413     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617414     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617415     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617416     3  0.0000      0.756 0.000 0.000 1.00
#&gt; SRR1617417     3  0.0000      0.756 0.000 0.000 1.00
#&gt; SRR1617418     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617419     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617420     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617421     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617422     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617423     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617424     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617425     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617427     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617426     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617428     3  0.4555      0.717 0.200 0.000 0.80
#&gt; SRR1617429     3  0.4555      0.717 0.200 0.000 0.80
#&gt; SRR1617432     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617433     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617434     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617436     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617435     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617437     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617438     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617439     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617440     2  0.1529      0.909 0.040 0.960 0.00
#&gt; SRR1617441     2  0.1529      0.909 0.040 0.960 0.00
#&gt; SRR1617443     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617442     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617444     2  0.4887      0.608 0.228 0.772 0.00
#&gt; SRR1617445     2  0.4887      0.608 0.228 0.772 0.00
#&gt; SRR1617446     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617447     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617448     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617449     1  0.0000      1.000 1.000 0.000 0.00
#&gt; SRR1617451     2  0.0000      0.945 0.000 1.000 0.00
#&gt; SRR1617450     2  0.0000      0.945 0.000 1.000 0.00
#&gt; SRR1617452     3  0.5706      0.553 0.000 0.320 0.68
#&gt; SRR1617454     2  0.0000      0.945 0.000 1.000 0.00
#&gt; SRR1617453     3  0.5706      0.553 0.000 0.320 0.68
#&gt; SRR1617456     2  0.0000      0.945 0.000 1.000 0.00
#&gt; SRR1617457     2  0.0000      0.945 0.000 1.000 0.00
#&gt; SRR1617455     2  0.0000      0.945 0.000 1.000 0.00
#&gt; SRR1617458     2  0.0000      0.945 0.000 1.000 0.00
#&gt; SRR1617459     2  0.0000      0.945 0.000 1.000 0.00
#&gt; SRR1617460     2  0.0000      0.945 0.000 1.000 0.00
#&gt; SRR1617461     2  0.0000      0.945 0.000 1.000 0.00
#&gt; SRR1617463     2  0.0000      0.945 0.000 1.000 0.00
#&gt; SRR1617462     2  0.0000      0.945 0.000 1.000 0.00
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3   p4
#&gt; SRR1617430     2  0.0469      0.946 0.000 0.988 0.012 0.00
#&gt; SRR1617431     2  0.0469      0.946 0.000 0.988 0.012 0.00
#&gt; SRR1617410     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617411     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617412     3  0.0000      1.000 0.000 0.000 1.000 0.00
#&gt; SRR1617413     3  0.0000      1.000 0.000 0.000 1.000 0.00
#&gt; SRR1617414     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617415     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617416     4  0.0000      0.730 0.000 0.000 0.000 1.00
#&gt; SRR1617417     4  0.0000      0.730 0.000 0.000 0.000 1.00
#&gt; SRR1617418     3  0.0000      1.000 0.000 0.000 1.000 0.00
#&gt; SRR1617419     3  0.0000      1.000 0.000 0.000 1.000 0.00
#&gt; SRR1617420     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617421     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617422     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617423     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617424     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617425     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617427     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617426     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617428     4  0.3610      0.693 0.200 0.000 0.000 0.80
#&gt; SRR1617429     4  0.3610      0.693 0.200 0.000 0.000 0.80
#&gt; SRR1617432     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617433     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617434     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617436     3  0.0000      1.000 0.000 0.000 1.000 0.00
#&gt; SRR1617435     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617437     3  0.0000      1.000 0.000 0.000 1.000 0.00
#&gt; SRR1617438     3  0.0000      1.000 0.000 0.000 1.000 0.00
#&gt; SRR1617439     3  0.0000      1.000 0.000 0.000 1.000 0.00
#&gt; SRR1617440     2  0.1211      0.923 0.000 0.960 0.040 0.00
#&gt; SRR1617441     2  0.1211      0.923 0.000 0.960 0.040 0.00
#&gt; SRR1617443     3  0.0000      1.000 0.000 0.000 1.000 0.00
#&gt; SRR1617442     3  0.0000      1.000 0.000 0.000 1.000 0.00
#&gt; SRR1617444     2  0.4212      0.648 0.216 0.772 0.012 0.00
#&gt; SRR1617445     2  0.4212      0.648 0.216 0.772 0.012 0.00
#&gt; SRR1617446     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617447     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617448     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617449     1  0.0000      1.000 1.000 0.000 0.000 0.00
#&gt; SRR1617451     2  0.0000      0.954 0.000 1.000 0.000 0.00
#&gt; SRR1617450     2  0.0000      0.954 0.000 1.000 0.000 0.00
#&gt; SRR1617452     4  0.4522      0.564 0.000 0.320 0.000 0.68
#&gt; SRR1617454     2  0.0000      0.954 0.000 1.000 0.000 0.00
#&gt; SRR1617453     4  0.4522      0.564 0.000 0.320 0.000 0.68
#&gt; SRR1617456     2  0.0000      0.954 0.000 1.000 0.000 0.00
#&gt; SRR1617457     2  0.0000      0.954 0.000 1.000 0.000 0.00
#&gt; SRR1617455     2  0.0000      0.954 0.000 1.000 0.000 0.00
#&gt; SRR1617458     2  0.0000      0.954 0.000 1.000 0.000 0.00
#&gt; SRR1617459     2  0.0000      0.954 0.000 1.000 0.000 0.00
#&gt; SRR1617460     2  0.0000      0.954 0.000 1.000 0.000 0.00
#&gt; SRR1617461     2  0.0000      0.954 0.000 1.000 0.000 0.00
#&gt; SRR1617463     2  0.0000      0.954 0.000 1.000 0.000 0.00
#&gt; SRR1617462     2  0.0000      0.954 0.000 1.000 0.000 0.00
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     2   0.088      0.729 0.000 0.968 0.000 0.032 0.000
#&gt; SRR1617431     2   0.088      0.729 0.000 0.968 0.000 0.032 0.000
#&gt; SRR1617410     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617411     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617412     3   0.411      0.995 0.000 0.000 0.624 0.376 0.000
#&gt; SRR1617413     3   0.411      0.995 0.000 0.000 0.624 0.376 0.000
#&gt; SRR1617414     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617415     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617416     4   0.423      0.443 0.000 0.000 0.000 0.580 0.420
#&gt; SRR1617417     4   0.423      0.443 0.000 0.000 0.000 0.580 0.420
#&gt; SRR1617418     3   0.415      0.992 0.000 0.000 0.612 0.388 0.000
#&gt; SRR1617419     3   0.415      0.992 0.000 0.000 0.612 0.388 0.000
#&gt; SRR1617420     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617421     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617422     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617423     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617424     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617425     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617427     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617426     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617428     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617429     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617432     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617433     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617434     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617436     3   0.411      0.995 0.000 0.000 0.624 0.376 0.000
#&gt; SRR1617435     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617437     3   0.411      0.995 0.000 0.000 0.624 0.376 0.000
#&gt; SRR1617438     3   0.415      0.992 0.000 0.000 0.612 0.388 0.000
#&gt; SRR1617439     3   0.415      0.992 0.000 0.000 0.612 0.388 0.000
#&gt; SRR1617440     2   0.163      0.714 0.000 0.940 0.016 0.044 0.000
#&gt; SRR1617441     2   0.163      0.714 0.000 0.940 0.016 0.044 0.000
#&gt; SRR1617443     3   0.411      0.995 0.000 0.000 0.624 0.376 0.000
#&gt; SRR1617442     3   0.411      0.995 0.000 0.000 0.624 0.376 0.000
#&gt; SRR1617444     2   0.407      0.536 0.216 0.752 0.000 0.032 0.000
#&gt; SRR1617445     2   0.407      0.536 0.216 0.752 0.000 0.032 0.000
#&gt; SRR1617446     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617447     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617448     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617449     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617451     2   0.000      0.739 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617450     2   0.000      0.739 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617452     4   0.663      0.612 0.000 0.052 0.268 0.572 0.108
#&gt; SRR1617454     2   0.000      0.739 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617453     4   0.663      0.612 0.000 0.052 0.268 0.572 0.108
#&gt; SRR1617456     2   0.411      0.693 0.000 0.624 0.376 0.000 0.000
#&gt; SRR1617457     2   0.411      0.693 0.000 0.624 0.376 0.000 0.000
#&gt; SRR1617455     2   0.000      0.739 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617458     2   0.411      0.693 0.000 0.624 0.376 0.000 0.000
#&gt; SRR1617459     2   0.411      0.693 0.000 0.624 0.376 0.000 0.000
#&gt; SRR1617460     2   0.411      0.693 0.000 0.624 0.376 0.000 0.000
#&gt; SRR1617461     2   0.411      0.693 0.000 0.624 0.376 0.000 0.000
#&gt; SRR1617463     2   0.411      0.693 0.000 0.624 0.376 0.000 0.000
#&gt; SRR1617462     2   0.411      0.693 0.000 0.624 0.376 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.0632      0.896 0.000 0.976 0.000 0.000 0.000 0.024
#&gt; SRR1617431     2  0.0632      0.896 0.000 0.976 0.000 0.000 0.000 0.024
#&gt; SRR1617410     1  0.0000      0.987 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.987 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617412     3  0.0146      0.988 0.000 0.004 0.996 0.000 0.000 0.000
#&gt; SRR1617413     3  0.0146      0.988 0.000 0.004 0.996 0.000 0.000 0.000
#&gt; SRR1617414     1  0.0000      0.987 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617415     1  0.0000      0.987 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617416     4  0.0632      0.653 0.000 0.000 0.000 0.976 0.024 0.000
#&gt; SRR1617417     4  0.0632      0.653 0.000 0.000 0.000 0.976 0.024 0.000
#&gt; SRR1617418     3  0.0547      0.987 0.000 0.020 0.980 0.000 0.000 0.000
#&gt; SRR1617419     3  0.0547      0.987 0.000 0.020 0.980 0.000 0.000 0.000
#&gt; SRR1617420     1  0.0000      0.987 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617421     1  0.0000      0.987 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617422     1  0.0632      0.987 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1617423     1  0.0632      0.987 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1617424     1  0.0632      0.987 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1617425     1  0.0632      0.987 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1617427     1  0.0632      0.987 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1617426     1  0.0632      0.987 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1617428     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617429     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617432     1  0.0000      0.987 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617433     1  0.0000      0.987 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617434     1  0.0000      0.987 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617436     3  0.0146      0.990 0.000 0.004 0.996 0.000 0.000 0.000
#&gt; SRR1617435     1  0.0000      0.987 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617437     3  0.0146      0.990 0.000 0.004 0.996 0.000 0.000 0.000
#&gt; SRR1617438     3  0.0547      0.987 0.000 0.020 0.980 0.000 0.000 0.000
#&gt; SRR1617439     3  0.0547      0.987 0.000 0.020 0.980 0.000 0.000 0.000
#&gt; SRR1617440     2  0.0260      0.884 0.000 0.992 0.008 0.000 0.000 0.000
#&gt; SRR1617441     2  0.0260      0.884 0.000 0.992 0.008 0.000 0.000 0.000
#&gt; SRR1617443     3  0.0000      0.990 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617442     3  0.0000      0.990 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617444     2  0.3473      0.671 0.192 0.780 0.000 0.024 0.000 0.004
#&gt; SRR1617445     2  0.3473      0.671 0.192 0.780 0.000 0.024 0.000 0.004
#&gt; SRR1617446     1  0.0632      0.987 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1617447     1  0.0632      0.987 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1617448     1  0.0632      0.987 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1617449     1  0.0632      0.987 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; SRR1617451     2  0.1204      0.896 0.000 0.944 0.000 0.000 0.000 0.056
#&gt; SRR1617450     2  0.1204      0.896 0.000 0.944 0.000 0.000 0.000 0.056
#&gt; SRR1617452     4  0.4211      0.680 0.000 0.008 0.000 0.660 0.020 0.312
#&gt; SRR1617454     2  0.1204      0.896 0.000 0.944 0.000 0.000 0.000 0.056
#&gt; SRR1617453     4  0.4211      0.680 0.000 0.008 0.000 0.660 0.020 0.312
#&gt; SRR1617456     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617457     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617455     2  0.1204      0.896 0.000 0.944 0.000 0.000 0.000 0.056
#&gt; SRR1617458     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617459     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617460     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617461     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617463     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617462     6  0.0000      1.000 0.000 0.000 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.201           0.630       0.761         0.4427 0.508   0.508
#> 3 3 0.255           0.726       0.764         0.3798 0.804   0.625
#> 4 4 0.496           0.727       0.770         0.1546 1.000   1.000
#> 5 5 0.590           0.310       0.664         0.0766 0.927   0.786
#> 6 6 0.635           0.563       0.673         0.0513 0.869   0.556
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2   0.760      0.577 0.220 0.780
#&gt; SRR1617431     2   0.760      0.577 0.220 0.780
#&gt; SRR1617410     1   0.921      0.705 0.664 0.336
#&gt; SRR1617411     1   0.921      0.705 0.664 0.336
#&gt; SRR1617412     1   0.584      0.487 0.860 0.140
#&gt; SRR1617413     1   0.584      0.487 0.860 0.140
#&gt; SRR1617414     1   0.943      0.681 0.640 0.360
#&gt; SRR1617415     1   0.943      0.681 0.640 0.360
#&gt; SRR1617416     1   0.886      0.602 0.696 0.304
#&gt; SRR1617417     1   0.886      0.602 0.696 0.304
#&gt; SRR1617418     1   0.644      0.467 0.836 0.164
#&gt; SRR1617419     1   0.644      0.467 0.836 0.164
#&gt; SRR1617420     1   0.876      0.706 0.704 0.296
#&gt; SRR1617421     1   0.876      0.706 0.704 0.296
#&gt; SRR1617422     1   0.988      0.515 0.564 0.436
#&gt; SRR1617423     1   0.988      0.515 0.564 0.436
#&gt; SRR1617424     1   0.917      0.703 0.668 0.332
#&gt; SRR1617425     1   0.917      0.703 0.668 0.332
#&gt; SRR1617427     1   0.891      0.706 0.692 0.308
#&gt; SRR1617426     1   0.891      0.706 0.692 0.308
#&gt; SRR1617428     2   0.921      0.456 0.336 0.664
#&gt; SRR1617429     2   0.921      0.456 0.336 0.664
#&gt; SRR1617432     1   0.932      0.699 0.652 0.348
#&gt; SRR1617433     1   0.932      0.699 0.652 0.348
#&gt; SRR1617434     1   0.917      0.702 0.668 0.332
#&gt; SRR1617436     1   0.662      0.489 0.828 0.172
#&gt; SRR1617435     1   0.917      0.702 0.668 0.332
#&gt; SRR1617437     1   0.662      0.489 0.828 0.172
#&gt; SRR1617438     1   0.634      0.470 0.840 0.160
#&gt; SRR1617439     1   0.634      0.470 0.840 0.160
#&gt; SRR1617440     2   0.994      0.414 0.456 0.544
#&gt; SRR1617441     2   0.994      0.414 0.456 0.544
#&gt; SRR1617443     1   0.625      0.468 0.844 0.156
#&gt; SRR1617442     1   0.625      0.468 0.844 0.156
#&gt; SRR1617444     2   0.802      0.576 0.244 0.756
#&gt; SRR1617445     2   0.802      0.576 0.244 0.756
#&gt; SRR1617446     1   0.913      0.705 0.672 0.328
#&gt; SRR1617447     1   0.913      0.705 0.672 0.328
#&gt; SRR1617448     1   0.917      0.702 0.668 0.332
#&gt; SRR1617449     1   0.917      0.702 0.668 0.332
#&gt; SRR1617451     2   0.242      0.771 0.040 0.960
#&gt; SRR1617450     2   0.242      0.771 0.040 0.960
#&gt; SRR1617452     2   0.697      0.638 0.188 0.812
#&gt; SRR1617454     2   0.242      0.771 0.040 0.960
#&gt; SRR1617453     2   0.697      0.638 0.188 0.812
#&gt; SRR1617456     2   0.343      0.767 0.064 0.936
#&gt; SRR1617457     2   0.343      0.767 0.064 0.936
#&gt; SRR1617455     2   0.242      0.771 0.040 0.960
#&gt; SRR1617458     2   0.358      0.763 0.068 0.932
#&gt; SRR1617459     2   0.358      0.763 0.068 0.932
#&gt; SRR1617460     2   0.443      0.729 0.092 0.908
#&gt; SRR1617461     2   0.443      0.729 0.092 0.908
#&gt; SRR1617463     2   0.443      0.729 0.092 0.908
#&gt; SRR1617462     2   0.443      0.729 0.092 0.908
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2   0.848      0.581 0.332 0.560 0.108
#&gt; SRR1617431     2   0.848      0.581 0.332 0.560 0.108
#&gt; SRR1617410     1   0.243      0.835 0.940 0.024 0.036
#&gt; SRR1617411     1   0.243      0.835 0.940 0.024 0.036
#&gt; SRR1617412     3   0.672      0.830 0.248 0.048 0.704
#&gt; SRR1617413     3   0.672      0.830 0.248 0.048 0.704
#&gt; SRR1617414     1   0.380      0.820 0.888 0.080 0.032
#&gt; SRR1617415     1   0.380      0.820 0.888 0.080 0.032
#&gt; SRR1617416     1   0.863      0.456 0.580 0.140 0.280
#&gt; SRR1617417     1   0.863      0.456 0.580 0.140 0.280
#&gt; SRR1617418     3   0.638      0.843 0.224 0.044 0.732
#&gt; SRR1617419     3   0.638      0.843 0.224 0.044 0.732
#&gt; SRR1617420     1   0.255      0.832 0.932 0.012 0.056
#&gt; SRR1617421     1   0.255      0.832 0.932 0.012 0.056
#&gt; SRR1617422     1   0.621      0.679 0.756 0.192 0.052
#&gt; SRR1617423     1   0.621      0.679 0.756 0.192 0.052
#&gt; SRR1617424     1   0.392      0.831 0.884 0.036 0.080
#&gt; SRR1617425     1   0.392      0.831 0.884 0.036 0.080
#&gt; SRR1617427     1   0.417      0.833 0.872 0.036 0.092
#&gt; SRR1617426     1   0.417      0.833 0.872 0.036 0.092
#&gt; SRR1617428     2   0.963      0.362 0.220 0.452 0.328
#&gt; SRR1617429     2   0.963      0.362 0.220 0.452 0.328
#&gt; SRR1617432     1   0.266      0.834 0.932 0.024 0.044
#&gt; SRR1617433     1   0.266      0.834 0.932 0.024 0.044
#&gt; SRR1617434     1   0.326      0.819 0.912 0.040 0.048
#&gt; SRR1617436     3   0.660      0.826 0.256 0.040 0.704
#&gt; SRR1617435     1   0.326      0.819 0.912 0.040 0.048
#&gt; SRR1617437     3   0.660      0.826 0.256 0.040 0.704
#&gt; SRR1617438     3   0.623      0.845 0.220 0.040 0.740
#&gt; SRR1617439     3   0.623      0.845 0.220 0.040 0.740
#&gt; SRR1617440     3   0.809      0.104 0.068 0.416 0.516
#&gt; SRR1617441     3   0.809      0.104 0.068 0.416 0.516
#&gt; SRR1617443     3   0.638      0.840 0.244 0.036 0.720
#&gt; SRR1617442     3   0.638      0.840 0.244 0.036 0.720
#&gt; SRR1617444     2   0.874      0.569 0.340 0.536 0.124
#&gt; SRR1617445     2   0.874      0.569 0.340 0.536 0.124
#&gt; SRR1617446     1   0.409      0.831 0.876 0.036 0.088
#&gt; SRR1617447     1   0.409      0.831 0.876 0.036 0.088
#&gt; SRR1617448     1   0.453      0.827 0.860 0.052 0.088
#&gt; SRR1617449     1   0.453      0.827 0.860 0.052 0.088
#&gt; SRR1617451     2   0.520      0.780 0.136 0.820 0.044
#&gt; SRR1617450     2   0.520      0.780 0.136 0.820 0.044
#&gt; SRR1617452     2   0.666      0.590 0.052 0.716 0.232
#&gt; SRR1617454     2   0.514      0.780 0.132 0.824 0.044
#&gt; SRR1617453     2   0.666      0.590 0.052 0.716 0.232
#&gt; SRR1617456     2   0.566      0.753 0.104 0.808 0.088
#&gt; SRR1617457     2   0.566      0.753 0.104 0.808 0.088
#&gt; SRR1617455     2   0.514      0.780 0.132 0.824 0.044
#&gt; SRR1617458     2   0.574      0.751 0.100 0.804 0.096
#&gt; SRR1617459     2   0.574      0.751 0.100 0.804 0.096
#&gt; SRR1617460     2   0.535      0.776 0.152 0.808 0.040
#&gt; SRR1617461     2   0.535      0.776 0.152 0.808 0.040
#&gt; SRR1617463     2   0.524      0.776 0.168 0.804 0.028
#&gt; SRR1617462     2   0.524      0.776 0.168 0.804 0.028
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2   0.731      0.644 0.164 0.652 0.080 0.104
#&gt; SRR1617431     2   0.731      0.644 0.164 0.652 0.080 0.104
#&gt; SRR1617410     1   0.256      0.798 0.920 0.008 0.032 0.040
#&gt; SRR1617411     1   0.256      0.798 0.920 0.008 0.032 0.040
#&gt; SRR1617412     3   0.402      0.854 0.068 0.000 0.836 0.096
#&gt; SRR1617413     3   0.402      0.854 0.068 0.000 0.836 0.096
#&gt; SRR1617414     1   0.372      0.789 0.872 0.048 0.024 0.056
#&gt; SRR1617415     1   0.372      0.789 0.872 0.048 0.024 0.056
#&gt; SRR1617416     1   0.740      0.492 0.460 0.044 0.060 0.436
#&gt; SRR1617417     1   0.740      0.492 0.460 0.044 0.060 0.436
#&gt; SRR1617418     3   0.324      0.869 0.064 0.000 0.880 0.056
#&gt; SRR1617419     3   0.324      0.869 0.064 0.000 0.880 0.056
#&gt; SRR1617420     1   0.264      0.791 0.908 0.000 0.060 0.032
#&gt; SRR1617421     1   0.264      0.791 0.908 0.000 0.060 0.032
#&gt; SRR1617422     1   0.665      0.723 0.688 0.124 0.036 0.152
#&gt; SRR1617423     1   0.665      0.723 0.688 0.124 0.036 0.152
#&gt; SRR1617424     1   0.518      0.799 0.780 0.028 0.048 0.144
#&gt; SRR1617425     1   0.518      0.799 0.780 0.028 0.048 0.144
#&gt; SRR1617427     1   0.509      0.805 0.784 0.024 0.048 0.144
#&gt; SRR1617426     1   0.509      0.805 0.784 0.024 0.048 0.144
#&gt; SRR1617428     2   0.920      0.412 0.120 0.388 0.152 0.340
#&gt; SRR1617429     2   0.920      0.412 0.120 0.388 0.152 0.340
#&gt; SRR1617432     1   0.322      0.796 0.892 0.016 0.032 0.060
#&gt; SRR1617433     1   0.322      0.796 0.892 0.016 0.032 0.060
#&gt; SRR1617434     1   0.333      0.781 0.880 0.004 0.044 0.072
#&gt; SRR1617436     3   0.320      0.859 0.072 0.008 0.888 0.032
#&gt; SRR1617435     1   0.333      0.781 0.880 0.004 0.044 0.072
#&gt; SRR1617437     3   0.320      0.859 0.072 0.008 0.888 0.032
#&gt; SRR1617438     3   0.270      0.870 0.068 0.000 0.904 0.028
#&gt; SRR1617439     3   0.270      0.870 0.068 0.000 0.904 0.028
#&gt; SRR1617440     3   0.739      0.303 0.016 0.312 0.544 0.128
#&gt; SRR1617441     3   0.739      0.303 0.016 0.312 0.544 0.128
#&gt; SRR1617443     3   0.281      0.865 0.080 0.000 0.896 0.024
#&gt; SRR1617442     3   0.281      0.865 0.080 0.000 0.896 0.024
#&gt; SRR1617444     2   0.845      0.553 0.212 0.540 0.092 0.156
#&gt; SRR1617445     2   0.845      0.553 0.212 0.540 0.092 0.156
#&gt; SRR1617446     1   0.542      0.794 0.756 0.020 0.056 0.168
#&gt; SRR1617447     1   0.542      0.794 0.756 0.020 0.056 0.168
#&gt; SRR1617448     1   0.620      0.779 0.708 0.040 0.060 0.192
#&gt; SRR1617449     1   0.620      0.779 0.708 0.040 0.060 0.192
#&gt; SRR1617451     2   0.353      0.752 0.044 0.876 0.012 0.068
#&gt; SRR1617450     2   0.353      0.752 0.044 0.876 0.012 0.068
#&gt; SRR1617452     2   0.684      0.609 0.028 0.556 0.052 0.364
#&gt; SRR1617454     2   0.259      0.757 0.036 0.920 0.012 0.032
#&gt; SRR1617453     2   0.684      0.609 0.028 0.556 0.052 0.364
#&gt; SRR1617456     2   0.460      0.716 0.012 0.796 0.032 0.160
#&gt; SRR1617457     2   0.460      0.716 0.012 0.796 0.032 0.160
#&gt; SRR1617455     2   0.259      0.757 0.036 0.920 0.012 0.032
#&gt; SRR1617458     2   0.478      0.713 0.012 0.780 0.032 0.176
#&gt; SRR1617459     2   0.478      0.713 0.012 0.780 0.032 0.176
#&gt; SRR1617460     2   0.469      0.753 0.052 0.804 0.012 0.132
#&gt; SRR1617461     2   0.469      0.753 0.052 0.804 0.012 0.132
#&gt; SRR1617463     2   0.378      0.756 0.056 0.864 0.012 0.068
#&gt; SRR1617462     2   0.378      0.756 0.056 0.864 0.012 0.068
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     2  0.6535     0.2467 0.060 0.668 0.044 0.068 0.160
#&gt; SRR1617431     2  0.6535     0.2467 0.060 0.668 0.044 0.068 0.160
#&gt; SRR1617410     1  0.1699     0.4189 0.944 0.008 0.036 0.004 0.008
#&gt; SRR1617411     1  0.1699     0.4189 0.944 0.008 0.036 0.004 0.008
#&gt; SRR1617412     3  0.4759     0.7457 0.016 0.004 0.760 0.068 0.152
#&gt; SRR1617413     3  0.4759     0.7457 0.016 0.004 0.760 0.068 0.152
#&gt; SRR1617414     1  0.5187     0.3338 0.752 0.048 0.008 0.060 0.132
#&gt; SRR1617415     1  0.5187     0.3338 0.752 0.048 0.008 0.060 0.132
#&gt; SRR1617416     1  0.7620     0.0523 0.388 0.004 0.040 0.328 0.240
#&gt; SRR1617417     1  0.7620     0.0523 0.388 0.004 0.040 0.328 0.240
#&gt; SRR1617418     3  0.3413     0.7951 0.008 0.008 0.852 0.028 0.104
#&gt; SRR1617419     3  0.3413     0.7951 0.008 0.008 0.852 0.028 0.104
#&gt; SRR1617420     1  0.3460     0.3978 0.860 0.004 0.076 0.024 0.036
#&gt; SRR1617421     1  0.3460     0.3978 0.860 0.004 0.076 0.024 0.036
#&gt; SRR1617422     1  0.6967    -0.5089 0.424 0.120 0.008 0.028 0.420
#&gt; SRR1617423     1  0.6967    -0.5089 0.424 0.120 0.008 0.028 0.420
#&gt; SRR1617424     1  0.5979    -0.6406 0.540 0.024 0.036 0.012 0.388
#&gt; SRR1617425     1  0.5979    -0.6406 0.540 0.024 0.036 0.012 0.388
#&gt; SRR1617427     1  0.6268    -0.3515 0.572 0.016 0.036 0.044 0.332
#&gt; SRR1617426     1  0.6268    -0.3515 0.572 0.016 0.036 0.044 0.332
#&gt; SRR1617428     4  0.8533     0.3780 0.064 0.348 0.084 0.384 0.120
#&gt; SRR1617429     4  0.8533     0.3780 0.064 0.348 0.084 0.384 0.120
#&gt; SRR1617432     1  0.3142     0.4100 0.876 0.004 0.012 0.048 0.060
#&gt; SRR1617433     1  0.3142     0.4100 0.876 0.004 0.012 0.048 0.060
#&gt; SRR1617434     1  0.2061     0.4211 0.928 0.004 0.040 0.024 0.004
#&gt; SRR1617436     3  0.4184     0.7725 0.016 0.012 0.820 0.072 0.080
#&gt; SRR1617435     1  0.2061     0.4211 0.928 0.004 0.040 0.024 0.004
#&gt; SRR1617437     3  0.4184     0.7725 0.016 0.012 0.820 0.072 0.080
#&gt; SRR1617438     3  0.2082     0.8018 0.012 0.004 0.928 0.012 0.044
#&gt; SRR1617439     3  0.2082     0.8018 0.012 0.004 0.928 0.012 0.044
#&gt; SRR1617440     3  0.7312     0.2871 0.000 0.264 0.516 0.096 0.124
#&gt; SRR1617441     3  0.7312     0.2871 0.000 0.264 0.516 0.096 0.124
#&gt; SRR1617443     3  0.1893     0.8013 0.024 0.000 0.936 0.012 0.028
#&gt; SRR1617442     3  0.1893     0.8013 0.024 0.000 0.936 0.012 0.028
#&gt; SRR1617444     2  0.8717     0.0120 0.108 0.372 0.084 0.092 0.344
#&gt; SRR1617445     2  0.8717     0.0120 0.108 0.372 0.084 0.092 0.344
#&gt; SRR1617446     1  0.6121    -0.8060 0.488 0.020 0.040 0.016 0.436
#&gt; SRR1617447     1  0.6121    -0.8060 0.488 0.020 0.040 0.016 0.436
#&gt; SRR1617448     5  0.6221     1.0000 0.436 0.036 0.040 0.008 0.480
#&gt; SRR1617449     5  0.6221     1.0000 0.436 0.036 0.040 0.008 0.480
#&gt; SRR1617451     2  0.2590     0.4756 0.012 0.908 0.008 0.028 0.044
#&gt; SRR1617450     2  0.2590     0.4756 0.012 0.908 0.008 0.028 0.044
#&gt; SRR1617452     4  0.6086     0.2407 0.032 0.404 0.032 0.520 0.012
#&gt; SRR1617454     2  0.0981     0.4998 0.012 0.972 0.008 0.000 0.008
#&gt; SRR1617453     4  0.6086     0.2407 0.032 0.404 0.032 0.520 0.012
#&gt; SRR1617456     2  0.5114     0.3626 0.000 0.708 0.032 0.216 0.044
#&gt; SRR1617457     2  0.5114     0.3626 0.000 0.708 0.032 0.216 0.044
#&gt; SRR1617455     2  0.0981     0.4998 0.012 0.972 0.008 0.000 0.008
#&gt; SRR1617458     2  0.5302     0.3507 0.000 0.688 0.036 0.232 0.044
#&gt; SRR1617459     2  0.5302     0.3507 0.000 0.688 0.036 0.232 0.044
#&gt; SRR1617460     2  0.5488     0.4077 0.028 0.696 0.000 0.184 0.092
#&gt; SRR1617461     2  0.5488     0.4077 0.028 0.696 0.000 0.184 0.092
#&gt; SRR1617463     2  0.4528     0.4545 0.024 0.784 0.000 0.108 0.084
#&gt; SRR1617462     2  0.4528     0.4545 0.024 0.784 0.000 0.108 0.084
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.6683     0.0518 0.160 0.620 0.020 0.064 0.040 0.096
#&gt; SRR1617431     2  0.6683     0.0518 0.160 0.620 0.020 0.064 0.040 0.096
#&gt; SRR1617410     5  0.2398     0.8144 0.080 0.000 0.000 0.028 0.888 0.004
#&gt; SRR1617411     5  0.2398     0.8144 0.080 0.000 0.000 0.028 0.888 0.004
#&gt; SRR1617412     3  0.4225     0.7029 0.028 0.000 0.696 0.000 0.012 0.264
#&gt; SRR1617413     3  0.4225     0.7029 0.028 0.000 0.696 0.000 0.012 0.264
#&gt; SRR1617414     5  0.4662     0.7108 0.096 0.028 0.000 0.008 0.748 0.120
#&gt; SRR1617415     5  0.4662     0.7108 0.096 0.028 0.000 0.008 0.748 0.120
#&gt; SRR1617416     4  0.7564     0.2445 0.304 0.000 0.024 0.328 0.276 0.068
#&gt; SRR1617417     4  0.7564     0.2445 0.304 0.000 0.024 0.328 0.276 0.068
#&gt; SRR1617418     3  0.2573     0.7638 0.000 0.000 0.856 0.004 0.008 0.132
#&gt; SRR1617419     3  0.2573     0.7638 0.000 0.000 0.856 0.004 0.008 0.132
#&gt; SRR1617420     5  0.3342     0.7924 0.084 0.000 0.012 0.020 0.848 0.036
#&gt; SRR1617421     5  0.3342     0.7924 0.084 0.000 0.012 0.020 0.848 0.036
#&gt; SRR1617422     1  0.6034     0.6436 0.588 0.044 0.012 0.016 0.288 0.052
#&gt; SRR1617423     1  0.6034     0.6436 0.588 0.044 0.012 0.016 0.288 0.052
#&gt; SRR1617424     1  0.4977     0.6661 0.640 0.020 0.024 0.008 0.300 0.008
#&gt; SRR1617425     1  0.4977     0.6661 0.640 0.020 0.024 0.008 0.300 0.008
#&gt; SRR1617427     1  0.5738     0.5108 0.540 0.012 0.012 0.008 0.360 0.068
#&gt; SRR1617426     1  0.5738     0.5108 0.540 0.012 0.012 0.008 0.360 0.068
#&gt; SRR1617428     6  0.8943     1.0000 0.100 0.244 0.052 0.244 0.060 0.300
#&gt; SRR1617429     6  0.8943     1.0000 0.100 0.244 0.052 0.244 0.060 0.300
#&gt; SRR1617432     5  0.2864     0.7936 0.028 0.000 0.000 0.012 0.860 0.100
#&gt; SRR1617433     5  0.2864     0.7936 0.028 0.000 0.000 0.012 0.860 0.100
#&gt; SRR1617434     5  0.1657     0.8136 0.056 0.000 0.000 0.016 0.928 0.000
#&gt; SRR1617436     3  0.3854     0.7218 0.036 0.000 0.804 0.016 0.016 0.128
#&gt; SRR1617435     5  0.1657     0.8136 0.056 0.000 0.000 0.016 0.928 0.000
#&gt; SRR1617437     3  0.3854     0.7218 0.036 0.000 0.804 0.016 0.016 0.128
#&gt; SRR1617438     3  0.1337     0.7726 0.008 0.000 0.956 0.012 0.008 0.016
#&gt; SRR1617439     3  0.1337     0.7726 0.008 0.000 0.956 0.012 0.008 0.016
#&gt; SRR1617440     3  0.7510     0.2535 0.080 0.192 0.508 0.128 0.000 0.092
#&gt; SRR1617441     3  0.7510     0.2535 0.080 0.192 0.508 0.128 0.000 0.092
#&gt; SRR1617443     3  0.2600     0.7723 0.020 0.000 0.892 0.008 0.020 0.060
#&gt; SRR1617442     3  0.2600     0.7723 0.020 0.000 0.892 0.008 0.020 0.060
#&gt; SRR1617444     1  0.8677    -0.1653 0.364 0.296 0.072 0.120 0.088 0.060
#&gt; SRR1617445     1  0.8677    -0.1653 0.364 0.296 0.072 0.120 0.088 0.060
#&gt; SRR1617446     1  0.4806     0.6759 0.688 0.008 0.028 0.012 0.248 0.016
#&gt; SRR1617447     1  0.4806     0.6759 0.688 0.008 0.028 0.012 0.248 0.016
#&gt; SRR1617448     1  0.4817     0.6807 0.708 0.016 0.028 0.012 0.220 0.016
#&gt; SRR1617449     1  0.4817     0.6807 0.708 0.016 0.028 0.012 0.220 0.016
#&gt; SRR1617451     2  0.3474     0.4417 0.052 0.852 0.004 0.044 0.012 0.036
#&gt; SRR1617450     2  0.3474     0.4417 0.052 0.852 0.004 0.044 0.012 0.036
#&gt; SRR1617452     4  0.4967     0.1427 0.016 0.276 0.016 0.660 0.024 0.008
#&gt; SRR1617454     2  0.0767     0.5301 0.012 0.976 0.000 0.000 0.008 0.004
#&gt; SRR1617453     4  0.4967     0.1427 0.016 0.276 0.016 0.660 0.024 0.008
#&gt; SRR1617456     2  0.5490     0.4571 0.012 0.596 0.008 0.288 0.000 0.096
#&gt; SRR1617457     2  0.5490     0.4571 0.012 0.596 0.008 0.288 0.000 0.096
#&gt; SRR1617455     2  0.0767     0.5301 0.012 0.976 0.000 0.000 0.008 0.004
#&gt; SRR1617458     2  0.5524     0.4390 0.012 0.572 0.008 0.320 0.000 0.088
#&gt; SRR1617459     2  0.5524     0.4390 0.012 0.572 0.008 0.320 0.000 0.088
#&gt; SRR1617460     2  0.6163     0.4724 0.060 0.580 0.004 0.276 0.016 0.064
#&gt; SRR1617461     2  0.6163     0.4724 0.060 0.580 0.004 0.276 0.016 0.064
#&gt; SRR1617463     2  0.5351     0.5018 0.060 0.692 0.004 0.176 0.008 0.060
#&gt; SRR1617462     2  0.5351     0.5018 0.060 0.692 0.004 0.176 0.008 0.060
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.992       0.996         0.5025 0.497   0.497
#> 3 3 0.952           0.958       0.980         0.3274 0.723   0.499
#> 4 4 0.801           0.812       0.881         0.1280 0.880   0.653
#> 5 5 0.816           0.792       0.870         0.0708 0.941   0.763
#> 6 6 0.819           0.733       0.817         0.0376 0.952   0.766
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2   0.000      0.991 0.000 1.000
#&gt; SRR1617431     2   0.000      0.991 0.000 1.000
#&gt; SRR1617410     1   0.000      1.000 1.000 0.000
#&gt; SRR1617411     1   0.000      1.000 1.000 0.000
#&gt; SRR1617412     1   0.000      1.000 1.000 0.000
#&gt; SRR1617413     1   0.000      1.000 1.000 0.000
#&gt; SRR1617414     1   0.000      1.000 1.000 0.000
#&gt; SRR1617415     1   0.000      1.000 1.000 0.000
#&gt; SRR1617416     1   0.000      1.000 1.000 0.000
#&gt; SRR1617417     1   0.000      1.000 1.000 0.000
#&gt; SRR1617418     1   0.000      1.000 1.000 0.000
#&gt; SRR1617419     1   0.000      1.000 1.000 0.000
#&gt; SRR1617420     1   0.000      1.000 1.000 0.000
#&gt; SRR1617421     1   0.000      1.000 1.000 0.000
#&gt; SRR1617422     2   0.482      0.889 0.104 0.896
#&gt; SRR1617423     2   0.482      0.889 0.104 0.896
#&gt; SRR1617424     1   0.000      1.000 1.000 0.000
#&gt; SRR1617425     1   0.000      1.000 1.000 0.000
#&gt; SRR1617427     1   0.000      1.000 1.000 0.000
#&gt; SRR1617426     1   0.000      1.000 1.000 0.000
#&gt; SRR1617428     2   0.000      0.991 0.000 1.000
#&gt; SRR1617429     2   0.000      0.991 0.000 1.000
#&gt; SRR1617432     1   0.000      1.000 1.000 0.000
#&gt; SRR1617433     1   0.000      1.000 1.000 0.000
#&gt; SRR1617434     1   0.000      1.000 1.000 0.000
#&gt; SRR1617436     1   0.000      1.000 1.000 0.000
#&gt; SRR1617435     1   0.000      1.000 1.000 0.000
#&gt; SRR1617437     1   0.000      1.000 1.000 0.000
#&gt; SRR1617438     1   0.000      1.000 1.000 0.000
#&gt; SRR1617439     1   0.000      1.000 1.000 0.000
#&gt; SRR1617440     2   0.000      0.991 0.000 1.000
#&gt; SRR1617441     2   0.000      0.991 0.000 1.000
#&gt; SRR1617443     1   0.000      1.000 1.000 0.000
#&gt; SRR1617442     1   0.000      1.000 1.000 0.000
#&gt; SRR1617444     2   0.000      0.991 0.000 1.000
#&gt; SRR1617445     2   0.000      0.991 0.000 1.000
#&gt; SRR1617446     1   0.000      1.000 1.000 0.000
#&gt; SRR1617447     1   0.000      1.000 1.000 0.000
#&gt; SRR1617448     1   0.000      1.000 1.000 0.000
#&gt; SRR1617449     1   0.000      1.000 1.000 0.000
#&gt; SRR1617451     2   0.000      0.991 0.000 1.000
#&gt; SRR1617450     2   0.000      0.991 0.000 1.000
#&gt; SRR1617452     2   0.000      0.991 0.000 1.000
#&gt; SRR1617454     2   0.000      0.991 0.000 1.000
#&gt; SRR1617453     2   0.000      0.991 0.000 1.000
#&gt; SRR1617456     2   0.000      0.991 0.000 1.000
#&gt; SRR1617457     2   0.000      0.991 0.000 1.000
#&gt; SRR1617455     2   0.000      0.991 0.000 1.000
#&gt; SRR1617458     2   0.000      0.991 0.000 1.000
#&gt; SRR1617459     2   0.000      0.991 0.000 1.000
#&gt; SRR1617460     2   0.000      0.991 0.000 1.000
#&gt; SRR1617461     2   0.000      0.991 0.000 1.000
#&gt; SRR1617463     2   0.000      0.991 0.000 1.000
#&gt; SRR1617462     2   0.000      0.991 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.0000      0.993 0.000 1.000 0.000
#&gt; SRR1617431     2  0.0000      0.993 0.000 1.000 0.000
#&gt; SRR1617410     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617412     3  0.0000      0.947 0.000 0.000 1.000
#&gt; SRR1617413     3  0.0000      0.947 0.000 0.000 1.000
#&gt; SRR1617414     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617415     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617416     1  0.3412      0.867 0.876 0.000 0.124
#&gt; SRR1617417     1  0.3412      0.867 0.876 0.000 0.124
#&gt; SRR1617418     3  0.0000      0.947 0.000 0.000 1.000
#&gt; SRR1617419     3  0.0000      0.947 0.000 0.000 1.000
#&gt; SRR1617420     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617421     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617422     1  0.0424      0.982 0.992 0.008 0.000
#&gt; SRR1617423     1  0.0424      0.982 0.992 0.008 0.000
#&gt; SRR1617424     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617428     3  0.5733      0.567 0.000 0.324 0.676
#&gt; SRR1617429     3  0.5733      0.567 0.000 0.324 0.676
#&gt; SRR1617432     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617433     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617434     1  0.0237      0.985 0.996 0.000 0.004
#&gt; SRR1617436     3  0.0237      0.944 0.004 0.000 0.996
#&gt; SRR1617435     1  0.0237      0.985 0.996 0.000 0.004
#&gt; SRR1617437     3  0.0237      0.944 0.004 0.000 0.996
#&gt; SRR1617438     3  0.0000      0.947 0.000 0.000 1.000
#&gt; SRR1617439     3  0.0000      0.947 0.000 0.000 1.000
#&gt; SRR1617440     3  0.0592      0.941 0.000 0.012 0.988
#&gt; SRR1617441     3  0.0592      0.941 0.000 0.012 0.988
#&gt; SRR1617443     3  0.0000      0.947 0.000 0.000 1.000
#&gt; SRR1617442     3  0.0000      0.947 0.000 0.000 1.000
#&gt; SRR1617444     2  0.1163      0.975 0.000 0.972 0.028
#&gt; SRR1617445     2  0.1163      0.975 0.000 0.972 0.028
#&gt; SRR1617446     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.987 1.000 0.000 0.000
#&gt; SRR1617451     2  0.0000      0.993 0.000 1.000 0.000
#&gt; SRR1617450     2  0.0000      0.993 0.000 1.000 0.000
#&gt; SRR1617452     2  0.0892      0.983 0.000 0.980 0.020
#&gt; SRR1617454     2  0.0000      0.993 0.000 1.000 0.000
#&gt; SRR1617453     2  0.0892      0.983 0.000 0.980 0.020
#&gt; SRR1617456     2  0.0237      0.992 0.000 0.996 0.004
#&gt; SRR1617457     2  0.0237      0.992 0.000 0.996 0.004
#&gt; SRR1617455     2  0.0000      0.993 0.000 1.000 0.000
#&gt; SRR1617458     2  0.0237      0.992 0.000 0.996 0.004
#&gt; SRR1617459     2  0.0237      0.992 0.000 0.996 0.004
#&gt; SRR1617460     2  0.0000      0.993 0.000 1.000 0.000
#&gt; SRR1617461     2  0.0000      0.993 0.000 1.000 0.000
#&gt; SRR1617463     2  0.0000      0.993 0.000 1.000 0.000
#&gt; SRR1617462     2  0.0000      0.993 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.3160      0.890 0.060 0.892 0.008 0.040
#&gt; SRR1617431     2  0.3160      0.890 0.060 0.892 0.008 0.040
#&gt; SRR1617410     4  0.3726      0.871 0.212 0.000 0.000 0.788
#&gt; SRR1617411     4  0.3726      0.871 0.212 0.000 0.000 0.788
#&gt; SRR1617412     3  0.0000      0.915 0.000 0.000 1.000 0.000
#&gt; SRR1617413     3  0.0000      0.915 0.000 0.000 1.000 0.000
#&gt; SRR1617414     4  0.4008      0.829 0.244 0.000 0.000 0.756
#&gt; SRR1617415     4  0.4008      0.829 0.244 0.000 0.000 0.756
#&gt; SRR1617416     4  0.5581      0.370 0.340 0.008 0.020 0.632
#&gt; SRR1617417     4  0.5581      0.370 0.340 0.008 0.020 0.632
#&gt; SRR1617418     3  0.0000      0.915 0.000 0.000 1.000 0.000
#&gt; SRR1617419     3  0.0000      0.915 0.000 0.000 1.000 0.000
#&gt; SRR1617420     4  0.4053      0.864 0.228 0.000 0.004 0.768
#&gt; SRR1617421     4  0.4053      0.864 0.228 0.000 0.004 0.768
#&gt; SRR1617422     1  0.1635      0.804 0.948 0.008 0.000 0.044
#&gt; SRR1617423     1  0.1635      0.804 0.948 0.008 0.000 0.044
#&gt; SRR1617424     1  0.1022      0.818 0.968 0.000 0.000 0.032
#&gt; SRR1617425     1  0.1022      0.818 0.968 0.000 0.000 0.032
#&gt; SRR1617427     1  0.1716      0.790 0.936 0.000 0.000 0.064
#&gt; SRR1617426     1  0.1716      0.790 0.936 0.000 0.000 0.064
#&gt; SRR1617428     3  0.8397      0.314 0.028 0.244 0.440 0.288
#&gt; SRR1617429     3  0.8397      0.314 0.028 0.244 0.440 0.288
#&gt; SRR1617432     4  0.3610      0.871 0.200 0.000 0.000 0.800
#&gt; SRR1617433     4  0.3610      0.871 0.200 0.000 0.000 0.800
#&gt; SRR1617434     4  0.3311      0.854 0.172 0.000 0.000 0.828
#&gt; SRR1617436     3  0.0000      0.915 0.000 0.000 1.000 0.000
#&gt; SRR1617435     4  0.3311      0.854 0.172 0.000 0.000 0.828
#&gt; SRR1617437     3  0.0000      0.915 0.000 0.000 1.000 0.000
#&gt; SRR1617438     3  0.0000      0.915 0.000 0.000 1.000 0.000
#&gt; SRR1617439     3  0.0000      0.915 0.000 0.000 1.000 0.000
#&gt; SRR1617440     3  0.1004      0.899 0.000 0.024 0.972 0.004
#&gt; SRR1617441     3  0.1004      0.899 0.000 0.024 0.972 0.004
#&gt; SRR1617443     3  0.0000      0.915 0.000 0.000 1.000 0.000
#&gt; SRR1617442     3  0.0000      0.915 0.000 0.000 1.000 0.000
#&gt; SRR1617444     1  0.6768      0.103 0.480 0.452 0.024 0.044
#&gt; SRR1617445     1  0.6768      0.103 0.480 0.452 0.024 0.044
#&gt; SRR1617446     1  0.0707      0.820 0.980 0.000 0.000 0.020
#&gt; SRR1617447     1  0.0707      0.820 0.980 0.000 0.000 0.020
#&gt; SRR1617448     1  0.0469      0.820 0.988 0.000 0.000 0.012
#&gt; SRR1617449     1  0.0469      0.820 0.988 0.000 0.000 0.012
#&gt; SRR1617451     2  0.1488      0.932 0.012 0.956 0.000 0.032
#&gt; SRR1617450     2  0.1488      0.932 0.012 0.956 0.000 0.032
#&gt; SRR1617452     2  0.4011      0.799 0.000 0.784 0.008 0.208
#&gt; SRR1617454     2  0.0817      0.939 0.000 0.976 0.000 0.024
#&gt; SRR1617453     2  0.4011      0.799 0.000 0.784 0.008 0.208
#&gt; SRR1617456     2  0.0592      0.941 0.000 0.984 0.000 0.016
#&gt; SRR1617457     2  0.0592      0.941 0.000 0.984 0.000 0.016
#&gt; SRR1617455     2  0.0817      0.939 0.000 0.976 0.000 0.024
#&gt; SRR1617458     2  0.0592      0.941 0.000 0.984 0.000 0.016
#&gt; SRR1617459     2  0.0592      0.941 0.000 0.984 0.000 0.016
#&gt; SRR1617460     2  0.1151      0.939 0.008 0.968 0.000 0.024
#&gt; SRR1617461     2  0.1151      0.939 0.008 0.968 0.000 0.024
#&gt; SRR1617463     2  0.0927      0.941 0.008 0.976 0.000 0.016
#&gt; SRR1617462     2  0.0927      0.941 0.008 0.976 0.000 0.016
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     2  0.3835      0.602 0.020 0.800 0.004 0.168 0.008
#&gt; SRR1617431     2  0.3835      0.602 0.020 0.800 0.004 0.168 0.008
#&gt; SRR1617410     5  0.0579      0.965 0.008 0.000 0.000 0.008 0.984
#&gt; SRR1617411     5  0.0579      0.965 0.008 0.000 0.000 0.008 0.984
#&gt; SRR1617412     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617413     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617414     5  0.2170      0.930 0.020 0.020 0.000 0.036 0.924
#&gt; SRR1617415     5  0.2170      0.930 0.020 0.020 0.000 0.036 0.924
#&gt; SRR1617416     4  0.5934      0.626 0.156 0.000 0.020 0.648 0.176
#&gt; SRR1617417     4  0.5934      0.626 0.156 0.000 0.020 0.648 0.176
#&gt; SRR1617418     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617419     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617420     5  0.0771      0.964 0.020 0.000 0.004 0.000 0.976
#&gt; SRR1617421     5  0.0771      0.964 0.020 0.000 0.004 0.000 0.976
#&gt; SRR1617422     1  0.1967      0.846 0.932 0.012 0.000 0.020 0.036
#&gt; SRR1617423     1  0.1967      0.846 0.932 0.012 0.000 0.020 0.036
#&gt; SRR1617424     1  0.1205      0.852 0.956 0.000 0.000 0.004 0.040
#&gt; SRR1617425     1  0.1205      0.852 0.956 0.000 0.000 0.004 0.040
#&gt; SRR1617427     1  0.2513      0.806 0.876 0.000 0.000 0.008 0.116
#&gt; SRR1617426     1  0.2513      0.806 0.876 0.000 0.000 0.008 0.116
#&gt; SRR1617428     4  0.5468      0.581 0.008 0.236 0.068 0.676 0.012
#&gt; SRR1617429     4  0.5468      0.581 0.008 0.236 0.068 0.676 0.012
#&gt; SRR1617432     5  0.0693      0.966 0.008 0.000 0.000 0.012 0.980
#&gt; SRR1617433     5  0.0693      0.966 0.008 0.000 0.000 0.012 0.980
#&gt; SRR1617434     5  0.0794      0.956 0.000 0.000 0.000 0.028 0.972
#&gt; SRR1617436     3  0.0162      0.961 0.000 0.000 0.996 0.004 0.000
#&gt; SRR1617435     5  0.0794      0.956 0.000 0.000 0.000 0.028 0.972
#&gt; SRR1617437     3  0.0162      0.961 0.000 0.000 0.996 0.004 0.000
#&gt; SRR1617438     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617439     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617440     3  0.4057      0.798 0.008 0.068 0.812 0.108 0.004
#&gt; SRR1617441     3  0.4057      0.798 0.008 0.068 0.812 0.108 0.004
#&gt; SRR1617443     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617442     3  0.0000      0.963 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617444     1  0.6192      0.262 0.496 0.124 0.000 0.376 0.004
#&gt; SRR1617445     1  0.6192      0.262 0.496 0.124 0.000 0.376 0.004
#&gt; SRR1617446     1  0.1117      0.851 0.964 0.000 0.000 0.016 0.020
#&gt; SRR1617447     1  0.1117      0.851 0.964 0.000 0.000 0.016 0.020
#&gt; SRR1617448     1  0.0992      0.847 0.968 0.000 0.000 0.024 0.008
#&gt; SRR1617449     1  0.0992      0.847 0.968 0.000 0.000 0.024 0.008
#&gt; SRR1617451     2  0.1952      0.699 0.004 0.912 0.000 0.084 0.000
#&gt; SRR1617450     2  0.1952      0.699 0.004 0.912 0.000 0.084 0.000
#&gt; SRR1617452     4  0.3210      0.480 0.000 0.212 0.000 0.788 0.000
#&gt; SRR1617454     2  0.0000      0.738 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617453     4  0.3210      0.480 0.000 0.212 0.000 0.788 0.000
#&gt; SRR1617456     2  0.3844      0.710 0.004 0.736 0.000 0.256 0.004
#&gt; SRR1617457     2  0.3844      0.710 0.004 0.736 0.000 0.256 0.004
#&gt; SRR1617455     2  0.0000      0.738 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617458     2  0.3895      0.706 0.004 0.728 0.000 0.264 0.004
#&gt; SRR1617459     2  0.3895      0.706 0.004 0.728 0.000 0.264 0.004
#&gt; SRR1617460     2  0.4213      0.664 0.012 0.680 0.000 0.308 0.000
#&gt; SRR1617461     2  0.4213      0.664 0.012 0.680 0.000 0.308 0.000
#&gt; SRR1617463     2  0.3039      0.725 0.012 0.836 0.000 0.152 0.000
#&gt; SRR1617462     2  0.3039      0.725 0.012 0.836 0.000 0.152 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.2989     0.3906 0.008 0.812 0.000 0.004 0.000 0.176
#&gt; SRR1617431     2  0.2989     0.3906 0.008 0.812 0.000 0.004 0.000 0.176
#&gt; SRR1617410     5  0.0146     0.9399 0.000 0.000 0.000 0.004 0.996 0.000
#&gt; SRR1617411     5  0.0146     0.9399 0.000 0.000 0.000 0.004 0.996 0.000
#&gt; SRR1617412     3  0.0767     0.9121 0.000 0.012 0.976 0.008 0.004 0.000
#&gt; SRR1617413     3  0.0767     0.9121 0.000 0.012 0.976 0.008 0.004 0.000
#&gt; SRR1617414     5  0.3135     0.9041 0.028 0.068 0.000 0.048 0.856 0.000
#&gt; SRR1617415     5  0.3135     0.9041 0.028 0.068 0.000 0.048 0.856 0.000
#&gt; SRR1617416     4  0.3261     0.7554 0.028 0.004 0.012 0.852 0.092 0.012
#&gt; SRR1617417     4  0.3261     0.7554 0.028 0.004 0.012 0.852 0.092 0.012
#&gt; SRR1617418     3  0.0622     0.9136 0.000 0.012 0.980 0.008 0.000 0.000
#&gt; SRR1617419     3  0.0622     0.9136 0.000 0.012 0.980 0.008 0.000 0.000
#&gt; SRR1617420     5  0.0146     0.9409 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; SRR1617421     5  0.0146     0.9409 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; SRR1617422     1  0.3268     0.8104 0.852 0.072 0.000 0.028 0.004 0.044
#&gt; SRR1617423     1  0.3268     0.8104 0.852 0.072 0.000 0.028 0.004 0.044
#&gt; SRR1617424     1  0.0260     0.8667 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; SRR1617425     1  0.0260     0.8667 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; SRR1617427     1  0.2493     0.8196 0.884 0.036 0.000 0.004 0.076 0.000
#&gt; SRR1617426     1  0.2493     0.8196 0.884 0.036 0.000 0.004 0.076 0.000
#&gt; SRR1617428     4  0.3633     0.6955 0.004 0.252 0.012 0.732 0.000 0.000
#&gt; SRR1617429     4  0.3633     0.6955 0.004 0.252 0.012 0.732 0.000 0.000
#&gt; SRR1617432     5  0.2614     0.9187 0.012 0.060 0.000 0.044 0.884 0.000
#&gt; SRR1617433     5  0.2614     0.9187 0.012 0.060 0.000 0.044 0.884 0.000
#&gt; SRR1617434     5  0.0632     0.9311 0.000 0.000 0.000 0.024 0.976 0.000
#&gt; SRR1617436     3  0.1349     0.8892 0.000 0.056 0.940 0.004 0.000 0.000
#&gt; SRR1617435     5  0.0632     0.9311 0.000 0.000 0.000 0.024 0.976 0.000
#&gt; SRR1617437     3  0.1349     0.8892 0.000 0.056 0.940 0.004 0.000 0.000
#&gt; SRR1617438     3  0.0260     0.9134 0.000 0.008 0.992 0.000 0.000 0.000
#&gt; SRR1617439     3  0.0260     0.9134 0.000 0.008 0.992 0.000 0.000 0.000
#&gt; SRR1617440     3  0.5460     0.5533 0.000 0.240 0.620 0.024 0.000 0.116
#&gt; SRR1617441     3  0.5460     0.5533 0.000 0.240 0.620 0.024 0.000 0.116
#&gt; SRR1617443     3  0.0146     0.9142 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1617442     3  0.0146     0.9142 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1617444     2  0.7595     0.2422 0.192 0.368 0.008 0.144 0.000 0.288
#&gt; SRR1617445     2  0.7595     0.2422 0.192 0.368 0.008 0.144 0.000 0.288
#&gt; SRR1617446     1  0.3104     0.8554 0.844 0.068 0.000 0.084 0.004 0.000
#&gt; SRR1617447     1  0.3104     0.8554 0.844 0.068 0.000 0.084 0.004 0.000
#&gt; SRR1617448     1  0.3104     0.8554 0.844 0.068 0.000 0.084 0.004 0.000
#&gt; SRR1617449     1  0.3104     0.8554 0.844 0.068 0.000 0.084 0.004 0.000
#&gt; SRR1617451     2  0.3695     0.0676 0.000 0.624 0.000 0.000 0.000 0.376
#&gt; SRR1617450     2  0.3695     0.0676 0.000 0.624 0.000 0.000 0.000 0.376
#&gt; SRR1617452     4  0.3380     0.7089 0.000 0.004 0.000 0.748 0.004 0.244
#&gt; SRR1617454     6  0.3838     0.3329 0.000 0.448 0.000 0.000 0.000 0.552
#&gt; SRR1617453     4  0.3380     0.7089 0.000 0.004 0.000 0.748 0.004 0.244
#&gt; SRR1617456     6  0.2263     0.6848 0.000 0.100 0.000 0.016 0.000 0.884
#&gt; SRR1617457     6  0.2263     0.6848 0.000 0.100 0.000 0.016 0.000 0.884
#&gt; SRR1617455     6  0.3838     0.3329 0.000 0.448 0.000 0.000 0.000 0.552
#&gt; SRR1617458     6  0.2263     0.6848 0.000 0.100 0.000 0.016 0.000 0.884
#&gt; SRR1617459     6  0.2263     0.6848 0.000 0.100 0.000 0.016 0.000 0.884
#&gt; SRR1617460     6  0.3039     0.6589 0.004 0.088 0.000 0.060 0.000 0.848
#&gt; SRR1617461     6  0.3039     0.6589 0.004 0.088 0.000 0.060 0.000 0.848
#&gt; SRR1617463     6  0.3758     0.6307 0.004 0.176 0.000 0.048 0.000 0.772
#&gt; SRR1617462     6  0.3758     0.6307 0.004 0.176 0.000 0.048 0.000 0.772
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.922           0.922       0.968         0.4748 0.525   0.525
#> 3 3 1.000           0.974       0.980         0.3646 0.793   0.616
#> 4 4 0.824           0.739       0.888         0.1034 0.978   0.935
#> 5 5 0.885           0.773       0.905         0.0883 0.925   0.767
#> 6 6 0.879           0.878       0.908         0.0207 0.947   0.791
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.0376      0.956 0.004 0.996
#&gt; SRR1617431     2  0.0376      0.956 0.004 0.996
#&gt; SRR1617410     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617411     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617412     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617413     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617414     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617415     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617416     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617417     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617418     1  0.2236      0.942 0.964 0.036
#&gt; SRR1617419     1  0.2603      0.936 0.956 0.044
#&gt; SRR1617420     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617421     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617422     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617423     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617424     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617425     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617427     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617426     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617428     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617429     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617432     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617433     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617434     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617436     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617435     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617437     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617438     1  0.9087      0.531 0.676 0.324
#&gt; SRR1617439     1  0.9170      0.513 0.668 0.332
#&gt; SRR1617440     2  0.9427      0.422 0.360 0.640
#&gt; SRR1617441     2  0.9427      0.422 0.360 0.640
#&gt; SRR1617443     1  0.5842      0.834 0.860 0.140
#&gt; SRR1617442     1  0.4562      0.885 0.904 0.096
#&gt; SRR1617444     1  0.1633      0.952 0.976 0.024
#&gt; SRR1617445     1  0.1843      0.949 0.972 0.028
#&gt; SRR1617446     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617447     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617448     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617449     1  0.0000      0.967 1.000 0.000
#&gt; SRR1617451     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617450     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617452     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617454     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617453     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617456     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617457     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617455     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617458     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617459     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617460     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617461     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617463     2  0.0000      0.959 0.000 1.000
#&gt; SRR1617462     2  0.0000      0.959 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2   0.429      0.777 0.180 0.820 0.000
#&gt; SRR1617431     2   0.400      0.802 0.160 0.840 0.000
#&gt; SRR1617410     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617411     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617412     3   0.164      0.990 0.044 0.000 0.956
#&gt; SRR1617413     3   0.164      0.990 0.044 0.000 0.956
#&gt; SRR1617414     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617415     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617416     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617417     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617418     3   0.164      0.990 0.044 0.000 0.956
#&gt; SRR1617419     3   0.164      0.990 0.044 0.000 0.956
#&gt; SRR1617420     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617421     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617422     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617423     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617424     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617425     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617427     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617426     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617428     2   0.000      0.963 0.000 1.000 0.000
#&gt; SRR1617429     2   0.000      0.963 0.000 1.000 0.000
#&gt; SRR1617432     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617433     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617434     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617436     3   0.164      0.990 0.044 0.000 0.956
#&gt; SRR1617435     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617437     3   0.164      0.990 0.044 0.000 0.956
#&gt; SRR1617438     3   0.164      0.990 0.044 0.000 0.956
#&gt; SRR1617439     3   0.164      0.990 0.044 0.000 0.956
#&gt; SRR1617440     3   0.000      0.950 0.000 0.000 1.000
#&gt; SRR1617441     3   0.000      0.950 0.000 0.000 1.000
#&gt; SRR1617443     3   0.164      0.990 0.044 0.000 0.956
#&gt; SRR1617442     3   0.164      0.990 0.044 0.000 0.956
#&gt; SRR1617444     1   0.103      0.973 0.976 0.024 0.000
#&gt; SRR1617445     1   0.129      0.965 0.968 0.032 0.000
#&gt; SRR1617446     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617447     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617448     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617449     1   0.000      0.997 1.000 0.000 0.000
#&gt; SRR1617451     2   0.000      0.963 0.000 1.000 0.000
#&gt; SRR1617450     2   0.000      0.963 0.000 1.000 0.000
#&gt; SRR1617452     2   0.164      0.954 0.000 0.956 0.044
#&gt; SRR1617454     2   0.000      0.963 0.000 1.000 0.000
#&gt; SRR1617453     2   0.164      0.954 0.000 0.956 0.044
#&gt; SRR1617456     2   0.164      0.954 0.000 0.956 0.044
#&gt; SRR1617457     2   0.164      0.954 0.000 0.956 0.044
#&gt; SRR1617455     2   0.000      0.963 0.000 1.000 0.000
#&gt; SRR1617458     2   0.164      0.954 0.000 0.956 0.044
#&gt; SRR1617459     2   0.164      0.954 0.000 0.956 0.044
#&gt; SRR1617460     2   0.000      0.963 0.000 1.000 0.000
#&gt; SRR1617461     2   0.000      0.963 0.000 1.000 0.000
#&gt; SRR1617463     2   0.000      0.963 0.000 1.000 0.000
#&gt; SRR1617462     2   0.000      0.963 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.1022      0.618 0.032 0.968 0.000 0.000
#&gt; SRR1617431     2  0.0921      0.623 0.028 0.972 0.000 0.000
#&gt; SRR1617410     1  0.3528      0.842 0.808 0.000 0.000 0.192
#&gt; SRR1617411     1  0.3528      0.842 0.808 0.000 0.000 0.192
#&gt; SRR1617412     3  0.0469      0.985 0.012 0.000 0.988 0.000
#&gt; SRR1617413     3  0.0469      0.985 0.012 0.000 0.988 0.000
#&gt; SRR1617414     1  0.4543      0.772 0.676 0.000 0.000 0.324
#&gt; SRR1617415     1  0.4543      0.772 0.676 0.000 0.000 0.324
#&gt; SRR1617416     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617417     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617418     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; SRR1617419     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; SRR1617420     1  0.1474      0.892 0.948 0.000 0.000 0.052
#&gt; SRR1617421     1  0.1716      0.889 0.936 0.000 0.000 0.064
#&gt; SRR1617422     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617428     2  0.0469      0.638 0.000 0.988 0.012 0.000
#&gt; SRR1617429     2  0.0592      0.635 0.000 0.984 0.016 0.000
#&gt; SRR1617432     1  0.4543      0.772 0.676 0.000 0.000 0.324
#&gt; SRR1617433     1  0.4543      0.772 0.676 0.000 0.000 0.324
#&gt; SRR1617434     1  0.4543      0.772 0.676 0.000 0.000 0.324
#&gt; SRR1617436     3  0.0188      0.994 0.004 0.000 0.996 0.000
#&gt; SRR1617435     1  0.4543      0.772 0.676 0.000 0.000 0.324
#&gt; SRR1617437     3  0.0188      0.994 0.004 0.000 0.996 0.000
#&gt; SRR1617438     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; SRR1617439     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; SRR1617440     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; SRR1617441     3  0.0188      0.993 0.000 0.004 0.996 0.000
#&gt; SRR1617443     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; SRR1617442     3  0.0000      0.996 0.000 0.000 1.000 0.000
#&gt; SRR1617444     1  0.0817      0.892 0.976 0.024 0.000 0.000
#&gt; SRR1617445     1  0.1022      0.888 0.968 0.032 0.000 0.000
#&gt; SRR1617446     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.902 1.000 0.000 0.000 0.000
#&gt; SRR1617451     2  0.0000      0.642 0.000 1.000 0.000 0.000
#&gt; SRR1617450     2  0.0000      0.642 0.000 1.000 0.000 0.000
#&gt; SRR1617452     4  0.4543      1.000 0.000 0.324 0.000 0.676
#&gt; SRR1617454     2  0.0000      0.642 0.000 1.000 0.000 0.000
#&gt; SRR1617453     4  0.4543      1.000 0.000 0.324 0.000 0.676
#&gt; SRR1617456     2  0.4972     -0.400 0.000 0.544 0.000 0.456
#&gt; SRR1617457     2  0.4972     -0.400 0.000 0.544 0.000 0.456
#&gt; SRR1617455     2  0.0000      0.642 0.000 1.000 0.000 0.000
#&gt; SRR1617458     2  0.4972     -0.400 0.000 0.544 0.000 0.456
#&gt; SRR1617459     2  0.4972     -0.400 0.000 0.544 0.000 0.456
#&gt; SRR1617460     2  0.3801      0.454 0.000 0.780 0.000 0.220
#&gt; SRR1617461     2  0.3801      0.454 0.000 0.780 0.000 0.220
#&gt; SRR1617463     2  0.3801      0.454 0.000 0.780 0.000 0.220
#&gt; SRR1617462     2  0.3801      0.454 0.000 0.780 0.000 0.220
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     2  0.1043      0.566 0.040 0.960 0.000 0.000 0.000
#&gt; SRR1617431     2  0.0794      0.571 0.028 0.972 0.000 0.000 0.000
#&gt; SRR1617410     1  0.3039      0.799 0.808 0.000 0.000 0.000 0.192
#&gt; SRR1617411     1  0.3039      0.799 0.808 0.000 0.000 0.000 0.192
#&gt; SRR1617412     3  0.0404      0.986 0.012 0.000 0.988 0.000 0.000
#&gt; SRR1617413     3  0.0404      0.986 0.012 0.000 0.988 0.000 0.000
#&gt; SRR1617414     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617415     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617416     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617417     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617418     3  0.0000      0.996 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617419     3  0.0000      0.996 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617420     1  0.1270      0.935 0.948 0.000 0.000 0.000 0.052
#&gt; SRR1617421     1  0.1478      0.927 0.936 0.000 0.000 0.000 0.064
#&gt; SRR1617422     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617428     2  0.4190      0.471 0.000 0.768 0.060 0.172 0.000
#&gt; SRR1617429     2  0.4252      0.469 0.000 0.764 0.064 0.172 0.000
#&gt; SRR1617432     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617433     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617434     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617436     3  0.0162      0.994 0.004 0.000 0.996 0.000 0.000
#&gt; SRR1617435     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617437     3  0.0162      0.994 0.004 0.000 0.996 0.000 0.000
#&gt; SRR1617438     3  0.0000      0.996 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617439     3  0.0000      0.996 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617440     3  0.0000      0.996 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617441     3  0.0162      0.993 0.000 0.004 0.996 0.000 0.000
#&gt; SRR1617443     3  0.0000      0.996 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617442     3  0.0000      0.996 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617444     1  0.1300      0.937 0.956 0.016 0.000 0.028 0.000
#&gt; SRR1617445     1  0.1493      0.930 0.948 0.024 0.000 0.028 0.000
#&gt; SRR1617446     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.965 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617451     2  0.1197      0.564 0.000 0.952 0.000 0.048 0.000
#&gt; SRR1617450     2  0.1197      0.564 0.000 0.952 0.000 0.048 0.000
#&gt; SRR1617452     4  0.2852      1.000 0.000 0.172 0.000 0.828 0.000
#&gt; SRR1617454     2  0.0000      0.572 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617453     4  0.2852      1.000 0.000 0.172 0.000 0.828 0.000
#&gt; SRR1617456     2  0.4304     -0.123 0.000 0.516 0.000 0.484 0.000
#&gt; SRR1617457     2  0.4304     -0.123 0.000 0.516 0.000 0.484 0.000
#&gt; SRR1617455     2  0.0000      0.572 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617458     2  0.4304     -0.123 0.000 0.516 0.000 0.484 0.000
#&gt; SRR1617459     2  0.4304     -0.123 0.000 0.516 0.000 0.484 0.000
#&gt; SRR1617460     2  0.3999      0.263 0.000 0.656 0.000 0.344 0.000
#&gt; SRR1617461     2  0.3999      0.263 0.000 0.656 0.000 0.344 0.000
#&gt; SRR1617463     2  0.3999      0.263 0.000 0.656 0.000 0.344 0.000
#&gt; SRR1617462     2  0.3999      0.263 0.000 0.656 0.000 0.344 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.5520      0.465 0.240 0.560 0.000 0.000 0.000 0.200
#&gt; SRR1617431     2  0.5083      0.571 0.164 0.632 0.000 0.000 0.000 0.204
#&gt; SRR1617410     1  0.2730      0.798 0.808 0.000 0.000 0.000 0.192 0.000
#&gt; SRR1617411     1  0.2730      0.798 0.808 0.000 0.000 0.000 0.192 0.000
#&gt; SRR1617412     3  0.0363      0.985 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; SRR1617413     3  0.0363      0.985 0.012 0.000 0.988 0.000 0.000 0.000
#&gt; SRR1617414     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617415     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617416     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617417     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617418     3  0.0000      0.995 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617419     3  0.0000      0.995 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617420     1  0.1075      0.928 0.952 0.000 0.000 0.000 0.048 0.000
#&gt; SRR1617421     1  0.1327      0.917 0.936 0.000 0.000 0.000 0.064 0.000
#&gt; SRR1617422     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617426     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617428     4  0.2762      1.000 0.000 0.196 0.000 0.804 0.000 0.000
#&gt; SRR1617429     4  0.2762      1.000 0.000 0.196 0.000 0.804 0.000 0.000
#&gt; SRR1617432     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617433     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617434     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617436     3  0.0146      0.993 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; SRR1617435     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617437     3  0.0146      0.993 0.004 0.000 0.996 0.000 0.000 0.000
#&gt; SRR1617438     3  0.0000      0.995 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617439     3  0.0000      0.995 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617440     3  0.0000      0.995 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617441     3  0.0146      0.993 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1617443     3  0.0000      0.995 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617442     3  0.0000      0.995 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617444     1  0.2092      0.859 0.876 0.000 0.000 0.000 0.000 0.124
#&gt; SRR1617445     1  0.2092      0.859 0.876 0.000 0.000 0.000 0.000 0.124
#&gt; SRR1617446     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.953 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1617451     2  0.3695      0.595 0.000 0.624 0.000 0.000 0.000 0.376
#&gt; SRR1617450     2  0.3695      0.595 0.000 0.624 0.000 0.000 0.000 0.376
#&gt; SRR1617452     6  0.5202      0.627 0.000 0.188 0.000 0.196 0.000 0.616
#&gt; SRR1617454     2  0.2762      0.697 0.000 0.804 0.000 0.000 0.000 0.196
#&gt; SRR1617453     6  0.5202      0.627 0.000 0.188 0.000 0.196 0.000 0.616
#&gt; SRR1617456     6  0.0000      0.832 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617457     6  0.0000      0.832 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617455     2  0.2762      0.697 0.000 0.804 0.000 0.000 0.000 0.196
#&gt; SRR1617458     6  0.0000      0.832 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617459     6  0.0000      0.832 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617460     2  0.0000      0.680 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617461     2  0.0000      0.680 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617463     2  0.0000      0.680 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617462     2  0.0000      0.680 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.755           0.917       0.939         0.4337 0.525   0.525
#> 3 3 0.759           0.901       0.890         0.4782 0.832   0.680
#> 4 4 0.716           0.820       0.900         0.1241 0.925   0.789
#> 5 5 0.641           0.617       0.749         0.0855 0.897   0.650
#> 6 6 0.743           0.688       0.799         0.0526 0.922   0.657
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617431     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617410     1  0.0000      0.870 1.000 0.000
#&gt; SRR1617411     1  0.0000      0.870 1.000 0.000
#&gt; SRR1617412     2  0.0000      0.966 0.000 1.000
#&gt; SRR1617413     2  0.0000      0.966 0.000 1.000
#&gt; SRR1617414     1  0.6343      0.851 0.840 0.160
#&gt; SRR1617415     1  0.6623      0.842 0.828 0.172
#&gt; SRR1617416     2  0.2778      0.978 0.048 0.952
#&gt; SRR1617417     2  0.2778      0.978 0.048 0.952
#&gt; SRR1617418     2  0.0000      0.966 0.000 1.000
#&gt; SRR1617419     2  0.0000      0.966 0.000 1.000
#&gt; SRR1617420     1  0.2423      0.880 0.960 0.040
#&gt; SRR1617421     1  0.2423      0.880 0.960 0.040
#&gt; SRR1617422     1  0.8016      0.769 0.756 0.244
#&gt; SRR1617423     1  0.8443      0.733 0.728 0.272
#&gt; SRR1617424     1  0.1633      0.878 0.976 0.024
#&gt; SRR1617425     1  0.1633      0.878 0.976 0.024
#&gt; SRR1617427     1  0.5519      0.867 0.872 0.128
#&gt; SRR1617426     1  0.5519      0.867 0.872 0.128
#&gt; SRR1617428     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617429     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617432     1  0.0672      0.873 0.992 0.008
#&gt; SRR1617433     1  0.0672      0.873 0.992 0.008
#&gt; SRR1617434     1  0.9795      0.448 0.584 0.416
#&gt; SRR1617436     2  0.0000      0.966 0.000 1.000
#&gt; SRR1617435     1  0.9795      0.448 0.584 0.416
#&gt; SRR1617437     2  0.0000      0.966 0.000 1.000
#&gt; SRR1617438     2  0.0000      0.966 0.000 1.000
#&gt; SRR1617439     2  0.0000      0.966 0.000 1.000
#&gt; SRR1617440     2  0.1414      0.976 0.020 0.980
#&gt; SRR1617441     2  0.1414      0.976 0.020 0.980
#&gt; SRR1617443     2  0.0000      0.966 0.000 1.000
#&gt; SRR1617442     2  0.0000      0.966 0.000 1.000
#&gt; SRR1617444     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617445     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617446     1  0.0000      0.870 1.000 0.000
#&gt; SRR1617447     1  0.0000      0.870 1.000 0.000
#&gt; SRR1617448     1  0.5842      0.863 0.860 0.140
#&gt; SRR1617449     1  0.5842      0.863 0.860 0.140
#&gt; SRR1617451     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617450     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617452     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617454     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617453     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617456     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617457     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617455     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617458     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617459     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617460     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617461     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617463     2  0.2423      0.983 0.040 0.960
#&gt; SRR1617462     2  0.2423      0.983 0.040 0.960
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.2796      0.861 0.092 0.908 0.000
#&gt; SRR1617431     2  0.2796      0.861 0.092 0.908 0.000
#&gt; SRR1617410     1  0.0000      0.983 1.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.983 1.000 0.000 0.000
#&gt; SRR1617412     3  0.0000      0.976 0.000 0.000 1.000
#&gt; SRR1617413     3  0.0000      0.976 0.000 0.000 1.000
#&gt; SRR1617414     1  0.1163      0.979 0.972 0.028 0.000
#&gt; SRR1617415     1  0.1163      0.979 0.972 0.028 0.000
#&gt; SRR1617416     2  0.4178      0.828 0.172 0.828 0.000
#&gt; SRR1617417     2  0.4178      0.828 0.172 0.828 0.000
#&gt; SRR1617418     3  0.0000      0.976 0.000 0.000 1.000
#&gt; SRR1617419     3  0.0000      0.976 0.000 0.000 1.000
#&gt; SRR1617420     1  0.0000      0.983 1.000 0.000 0.000
#&gt; SRR1617421     1  0.0000      0.983 1.000 0.000 0.000
#&gt; SRR1617422     1  0.1529      0.971 0.960 0.040 0.000
#&gt; SRR1617423     1  0.1529      0.971 0.960 0.040 0.000
#&gt; SRR1617424     1  0.0747      0.982 0.984 0.016 0.000
#&gt; SRR1617425     1  0.0747      0.982 0.984 0.016 0.000
#&gt; SRR1617427     1  0.0592      0.984 0.988 0.012 0.000
#&gt; SRR1617426     1  0.0592      0.984 0.988 0.012 0.000
#&gt; SRR1617428     2  0.3267      0.850 0.116 0.884 0.000
#&gt; SRR1617429     2  0.3267      0.850 0.116 0.884 0.000
#&gt; SRR1617432     1  0.0000      0.983 1.000 0.000 0.000
#&gt; SRR1617433     1  0.0000      0.983 1.000 0.000 0.000
#&gt; SRR1617434     1  0.0592      0.984 0.988 0.012 0.000
#&gt; SRR1617436     3  0.2796      0.896 0.000 0.092 0.908
#&gt; SRR1617435     1  0.0747      0.983 0.984 0.016 0.000
#&gt; SRR1617437     3  0.2796      0.896 0.000 0.092 0.908
#&gt; SRR1617438     3  0.0000      0.976 0.000 0.000 1.000
#&gt; SRR1617439     3  0.0000      0.976 0.000 0.000 1.000
#&gt; SRR1617440     2  0.6062      0.454 0.000 0.616 0.384
#&gt; SRR1617441     2  0.6062      0.454 0.000 0.616 0.384
#&gt; SRR1617443     3  0.0000      0.976 0.000 0.000 1.000
#&gt; SRR1617442     3  0.0000      0.976 0.000 0.000 1.000
#&gt; SRR1617444     2  0.2356      0.867 0.072 0.928 0.000
#&gt; SRR1617445     2  0.2356      0.867 0.072 0.928 0.000
#&gt; SRR1617446     1  0.0000      0.983 1.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.983 1.000 0.000 0.000
#&gt; SRR1617448     1  0.1411      0.974 0.964 0.036 0.000
#&gt; SRR1617449     1  0.1411      0.974 0.964 0.036 0.000
#&gt; SRR1617451     2  0.0000      0.874 0.000 1.000 0.000
#&gt; SRR1617450     2  0.0000      0.874 0.000 1.000 0.000
#&gt; SRR1617452     2  0.0000      0.874 0.000 1.000 0.000
#&gt; SRR1617454     2  0.0000      0.874 0.000 1.000 0.000
#&gt; SRR1617453     2  0.0000      0.874 0.000 1.000 0.000
#&gt; SRR1617456     2  0.0000      0.874 0.000 1.000 0.000
#&gt; SRR1617457     2  0.0000      0.874 0.000 1.000 0.000
#&gt; SRR1617455     2  0.0000      0.874 0.000 1.000 0.000
#&gt; SRR1617458     2  0.0000      0.874 0.000 1.000 0.000
#&gt; SRR1617459     2  0.0000      0.874 0.000 1.000 0.000
#&gt; SRR1617460     2  0.4887      0.748 0.228 0.772 0.000
#&gt; SRR1617461     2  0.4887      0.748 0.228 0.772 0.000
#&gt; SRR1617463     2  0.4887      0.748 0.228 0.772 0.000
#&gt; SRR1617462     2  0.4887      0.748 0.228 0.772 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.3521      0.739 0.084 0.864 0.000 0.052
#&gt; SRR1617431     2  0.3521      0.739 0.084 0.864 0.000 0.052
#&gt; SRR1617410     1  0.1118      0.916 0.964 0.000 0.000 0.036
#&gt; SRR1617411     1  0.1118      0.916 0.964 0.000 0.000 0.036
#&gt; SRR1617412     3  0.0000      0.968 0.000 0.000 1.000 0.000
#&gt; SRR1617413     3  0.0000      0.968 0.000 0.000 1.000 0.000
#&gt; SRR1617414     1  0.2124      0.901 0.924 0.068 0.000 0.008
#&gt; SRR1617415     1  0.2124      0.901 0.924 0.068 0.000 0.008
#&gt; SRR1617416     4  0.1059      0.774 0.012 0.016 0.000 0.972
#&gt; SRR1617417     4  0.1059      0.774 0.012 0.016 0.000 0.972
#&gt; SRR1617418     3  0.0000      0.968 0.000 0.000 1.000 0.000
#&gt; SRR1617419     3  0.0000      0.968 0.000 0.000 1.000 0.000
#&gt; SRR1617420     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617421     1  0.0000      0.919 1.000 0.000 0.000 0.000
#&gt; SRR1617422     1  0.3647      0.817 0.832 0.152 0.000 0.016
#&gt; SRR1617423     1  0.3647      0.817 0.832 0.152 0.000 0.016
#&gt; SRR1617424     1  0.1004      0.921 0.972 0.024 0.000 0.004
#&gt; SRR1617425     1  0.1004      0.921 0.972 0.024 0.000 0.004
#&gt; SRR1617427     1  0.1488      0.915 0.956 0.012 0.000 0.032
#&gt; SRR1617426     1  0.1488      0.915 0.956 0.012 0.000 0.032
#&gt; SRR1617428     4  0.6135      0.534 0.056 0.376 0.000 0.568
#&gt; SRR1617429     4  0.6135      0.534 0.056 0.376 0.000 0.568
#&gt; SRR1617432     1  0.1118      0.917 0.964 0.000 0.000 0.036
#&gt; SRR1617433     1  0.1118      0.917 0.964 0.000 0.000 0.036
#&gt; SRR1617434     1  0.3528      0.792 0.808 0.000 0.000 0.192
#&gt; SRR1617436     3  0.2593      0.863 0.000 0.104 0.892 0.004
#&gt; SRR1617435     1  0.4040      0.727 0.752 0.000 0.000 0.248
#&gt; SRR1617437     3  0.2593      0.863 0.000 0.104 0.892 0.004
#&gt; SRR1617438     3  0.0000      0.968 0.000 0.000 1.000 0.000
#&gt; SRR1617439     3  0.0000      0.968 0.000 0.000 1.000 0.000
#&gt; SRR1617440     2  0.5097      0.351 0.000 0.568 0.428 0.004
#&gt; SRR1617441     2  0.5097      0.351 0.000 0.568 0.428 0.004
#&gt; SRR1617443     3  0.0000      0.968 0.000 0.000 1.000 0.000
#&gt; SRR1617442     3  0.0000      0.968 0.000 0.000 1.000 0.000
#&gt; SRR1617444     2  0.2635      0.773 0.020 0.904 0.000 0.076
#&gt; SRR1617445     2  0.2635      0.773 0.020 0.904 0.000 0.076
#&gt; SRR1617446     1  0.0336      0.920 0.992 0.000 0.000 0.008
#&gt; SRR1617447     1  0.0336      0.920 0.992 0.000 0.000 0.008
#&gt; SRR1617448     1  0.2973      0.883 0.884 0.096 0.000 0.020
#&gt; SRR1617449     1  0.2861      0.884 0.888 0.096 0.000 0.016
#&gt; SRR1617451     2  0.0592      0.801 0.000 0.984 0.000 0.016
#&gt; SRR1617450     2  0.0592      0.801 0.000 0.984 0.000 0.016
#&gt; SRR1617452     4  0.2530      0.796 0.000 0.112 0.000 0.888
#&gt; SRR1617454     2  0.0937      0.802 0.012 0.976 0.000 0.012
#&gt; SRR1617453     4  0.2530      0.796 0.000 0.112 0.000 0.888
#&gt; SRR1617456     2  0.1637      0.796 0.000 0.940 0.000 0.060
#&gt; SRR1617457     2  0.1637      0.796 0.000 0.940 0.000 0.060
#&gt; SRR1617455     2  0.0937      0.802 0.012 0.976 0.000 0.012
#&gt; SRR1617458     2  0.1637      0.796 0.000 0.940 0.000 0.060
#&gt; SRR1617459     2  0.1637      0.796 0.000 0.940 0.000 0.060
#&gt; SRR1617460     2  0.3942      0.679 0.000 0.764 0.000 0.236
#&gt; SRR1617461     2  0.3942      0.679 0.000 0.764 0.000 0.236
#&gt; SRR1617463     2  0.3837      0.692 0.000 0.776 0.000 0.224
#&gt; SRR1617462     2  0.3837      0.692 0.000 0.776 0.000 0.224
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     2  0.6027      0.597 0.052 0.644 0.000 0.076 0.228
#&gt; SRR1617431     2  0.6027      0.597 0.052 0.644 0.000 0.076 0.228
#&gt; SRR1617410     1  0.0609      0.606 0.980 0.000 0.000 0.020 0.000
#&gt; SRR1617411     1  0.0609      0.606 0.980 0.000 0.000 0.020 0.000
#&gt; SRR1617412     3  0.0000      0.874 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617413     3  0.0000      0.874 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617414     5  0.5646      0.662 0.356 0.004 0.000 0.076 0.564
#&gt; SRR1617415     5  0.5646      0.662 0.356 0.004 0.000 0.076 0.564
#&gt; SRR1617416     4  0.3608      0.782 0.064 0.000 0.000 0.824 0.112
#&gt; SRR1617417     4  0.3608      0.782 0.064 0.000 0.000 0.824 0.112
#&gt; SRR1617418     3  0.0000      0.874 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617419     3  0.0000      0.874 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617420     1  0.5045      0.322 0.696 0.000 0.000 0.108 0.196
#&gt; SRR1617421     1  0.5045      0.322 0.696 0.000 0.000 0.108 0.196
#&gt; SRR1617422     5  0.4908      0.668 0.356 0.036 0.000 0.000 0.608
#&gt; SRR1617423     5  0.4908      0.668 0.356 0.036 0.000 0.000 0.608
#&gt; SRR1617424     1  0.5364     -0.190 0.572 0.000 0.000 0.064 0.364
#&gt; SRR1617425     1  0.5364     -0.190 0.572 0.000 0.000 0.064 0.364
#&gt; SRR1617427     5  0.6175      0.476 0.424 0.004 0.000 0.116 0.456
#&gt; SRR1617426     5  0.6142      0.470 0.428 0.004 0.000 0.112 0.456
#&gt; SRR1617428     4  0.5961      0.673 0.076 0.160 0.000 0.680 0.084
#&gt; SRR1617429     4  0.5961      0.673 0.076 0.160 0.000 0.680 0.084
#&gt; SRR1617432     1  0.0162      0.604 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1617433     1  0.0162      0.604 0.996 0.000 0.000 0.000 0.004
#&gt; SRR1617434     1  0.3662      0.439 0.744 0.000 0.000 0.252 0.004
#&gt; SRR1617436     3  0.2304      0.815 0.000 0.008 0.892 0.000 0.100
#&gt; SRR1617435     1  0.3766      0.424 0.728 0.000 0.000 0.268 0.004
#&gt; SRR1617437     3  0.2304      0.815 0.000 0.008 0.892 0.000 0.100
#&gt; SRR1617438     3  0.0000      0.874 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617439     3  0.0000      0.874 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617440     3  0.5843      0.220 0.000 0.388 0.512 0.000 0.100
#&gt; SRR1617441     3  0.5843      0.220 0.000 0.388 0.512 0.000 0.100
#&gt; SRR1617443     3  0.0000      0.874 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617442     3  0.0000      0.874 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1617444     2  0.5721      0.619 0.052 0.696 0.000 0.096 0.156
#&gt; SRR1617445     2  0.5721      0.619 0.052 0.696 0.000 0.096 0.156
#&gt; SRR1617446     1  0.3488      0.492 0.808 0.000 0.000 0.024 0.168
#&gt; SRR1617447     1  0.3656      0.493 0.800 0.000 0.000 0.032 0.168
#&gt; SRR1617448     5  0.6495      0.600 0.388 0.056 0.000 0.060 0.496
#&gt; SRR1617449     5  0.6398      0.593 0.396 0.056 0.000 0.052 0.496
#&gt; SRR1617451     2  0.1608      0.746 0.000 0.928 0.000 0.000 0.072
#&gt; SRR1617450     2  0.1608      0.746 0.000 0.928 0.000 0.000 0.072
#&gt; SRR1617452     4  0.0000      0.808 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617454     2  0.1908      0.745 0.000 0.908 0.000 0.000 0.092
#&gt; SRR1617453     4  0.0000      0.808 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617456     2  0.1764      0.737 0.000 0.928 0.000 0.008 0.064
#&gt; SRR1617457     2  0.1764      0.737 0.000 0.928 0.000 0.008 0.064
#&gt; SRR1617455     2  0.1908      0.745 0.000 0.908 0.000 0.000 0.092
#&gt; SRR1617458     2  0.1764      0.737 0.000 0.928 0.000 0.008 0.064
#&gt; SRR1617459     2  0.1764      0.737 0.000 0.928 0.000 0.008 0.064
#&gt; SRR1617460     2  0.5396      0.507 0.000 0.560 0.000 0.064 0.376
#&gt; SRR1617461     2  0.5396      0.507 0.000 0.560 0.000 0.064 0.376
#&gt; SRR1617463     2  0.5103      0.505 0.000 0.556 0.000 0.040 0.404
#&gt; SRR1617462     2  0.5103      0.505 0.000 0.556 0.000 0.040 0.404
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.3565      0.692 0.092 0.808 0.000 0.000 0.004 0.096
#&gt; SRR1617431     2  0.3565      0.692 0.092 0.808 0.000 0.000 0.004 0.096
#&gt; SRR1617410     5  0.2145      0.682 0.072 0.000 0.000 0.028 0.900 0.000
#&gt; SRR1617411     5  0.2145      0.682 0.072 0.000 0.000 0.028 0.900 0.000
#&gt; SRR1617412     3  0.0146      0.884 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1617413     3  0.0146      0.884 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1617414     1  0.1471      0.735 0.932 0.000 0.000 0.000 0.064 0.004
#&gt; SRR1617415     1  0.1471      0.735 0.932 0.000 0.000 0.000 0.064 0.004
#&gt; SRR1617416     4  0.0508      0.835 0.000 0.000 0.000 0.984 0.012 0.004
#&gt; SRR1617417     4  0.0508      0.835 0.000 0.000 0.000 0.984 0.012 0.004
#&gt; SRR1617418     3  0.0000      0.886 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617419     3  0.0000      0.886 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617420     1  0.3868      0.277 0.508 0.000 0.000 0.000 0.492 0.000
#&gt; SRR1617421     1  0.3998      0.269 0.504 0.000 0.000 0.004 0.492 0.000
#&gt; SRR1617422     1  0.1080      0.700 0.960 0.032 0.000 0.000 0.004 0.004
#&gt; SRR1617423     1  0.1080      0.700 0.960 0.032 0.000 0.000 0.004 0.004
#&gt; SRR1617424     1  0.3805      0.593 0.664 0.000 0.000 0.004 0.328 0.004
#&gt; SRR1617425     1  0.3805      0.593 0.664 0.000 0.000 0.004 0.328 0.004
#&gt; SRR1617427     1  0.1588      0.732 0.924 0.000 0.000 0.000 0.072 0.004
#&gt; SRR1617426     1  0.1588      0.732 0.924 0.000 0.000 0.000 0.072 0.004
#&gt; SRR1617428     4  0.5132      0.602 0.080 0.028 0.000 0.656 0.000 0.236
#&gt; SRR1617429     4  0.5132      0.602 0.080 0.028 0.000 0.656 0.000 0.236
#&gt; SRR1617432     5  0.1663      0.674 0.088 0.000 0.000 0.000 0.912 0.000
#&gt; SRR1617433     5  0.1663      0.674 0.088 0.000 0.000 0.000 0.912 0.000
#&gt; SRR1617434     5  0.3907      0.274 0.004 0.000 0.000 0.408 0.588 0.000
#&gt; SRR1617436     3  0.2911      0.826 0.044 0.060 0.876 0.008 0.008 0.004
#&gt; SRR1617435     5  0.3907      0.274 0.004 0.000 0.000 0.408 0.588 0.000
#&gt; SRR1617437     3  0.2911      0.826 0.044 0.060 0.876 0.008 0.008 0.004
#&gt; SRR1617438     3  0.0000      0.886 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617439     3  0.0000      0.886 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617440     3  0.5426      0.351 0.044 0.372 0.552 0.008 0.004 0.020
#&gt; SRR1617441     3  0.5426      0.351 0.044 0.372 0.552 0.008 0.004 0.020
#&gt; SRR1617443     3  0.0000      0.886 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617442     3  0.0000      0.886 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617444     2  0.5482      0.613 0.084 0.600 0.000 0.032 0.000 0.284
#&gt; SRR1617445     2  0.5482      0.613 0.084 0.600 0.000 0.032 0.000 0.284
#&gt; SRR1617446     5  0.3756      0.215 0.352 0.000 0.000 0.004 0.644 0.000
#&gt; SRR1617447     5  0.3756      0.215 0.352 0.000 0.000 0.004 0.644 0.000
#&gt; SRR1617448     1  0.4196      0.664 0.752 0.052 0.000 0.012 0.180 0.004
#&gt; SRR1617449     1  0.4100      0.667 0.756 0.052 0.000 0.008 0.180 0.004
#&gt; SRR1617451     2  0.3046      0.732 0.012 0.800 0.000 0.000 0.000 0.188
#&gt; SRR1617450     2  0.3046      0.732 0.012 0.800 0.000 0.000 0.000 0.188
#&gt; SRR1617452     4  0.0291      0.838 0.004 0.004 0.000 0.992 0.000 0.000
#&gt; SRR1617454     2  0.2768      0.737 0.012 0.832 0.000 0.000 0.000 0.156
#&gt; SRR1617453     4  0.0291      0.838 0.004 0.004 0.000 0.992 0.000 0.000
#&gt; SRR1617456     2  0.4550      0.648 0.000 0.676 0.000 0.000 0.084 0.240
#&gt; SRR1617457     2  0.4550      0.648 0.000 0.676 0.000 0.000 0.084 0.240
#&gt; SRR1617455     2  0.2768      0.737 0.012 0.832 0.000 0.000 0.000 0.156
#&gt; SRR1617458     2  0.4550      0.648 0.000 0.676 0.000 0.000 0.084 0.240
#&gt; SRR1617459     2  0.4550      0.648 0.000 0.676 0.000 0.000 0.084 0.240
#&gt; SRR1617460     6  0.1405      0.988 0.024 0.004 0.000 0.024 0.000 0.948
#&gt; SRR1617461     6  0.1405      0.988 0.024 0.004 0.000 0.024 0.000 0.948
#&gt; SRR1617463     6  0.1320      0.988 0.036 0.000 0.000 0.016 0.000 0.948
#&gt; SRR1617462     6  0.1320      0.988 0.036 0.000 0.000 0.016 0.000 0.948
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:NMF






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.753           0.914       0.955         0.4989 0.497   0.497
#> 3 3 0.807           0.878       0.949         0.3142 0.709   0.487
#> 4 4 0.777           0.819       0.874         0.1051 0.944   0.840
#> 5 5 0.895           0.881       0.919         0.0830 0.916   0.716
#> 6 6 0.851           0.791       0.854         0.0494 0.950   0.780
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617431     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617410     1  0.2603      0.942 0.956 0.044
#&gt; SRR1617411     1  0.2236      0.947 0.964 0.036
#&gt; SRR1617412     1  0.1414      0.959 0.980 0.020
#&gt; SRR1617413     1  0.1414      0.959 0.980 0.020
#&gt; SRR1617414     1  0.3274      0.931 0.940 0.060
#&gt; SRR1617415     1  0.3274      0.931 0.940 0.060
#&gt; SRR1617416     1  0.4690      0.893 0.900 0.100
#&gt; SRR1617417     1  0.5059      0.881 0.888 0.112
#&gt; SRR1617418     1  0.1414      0.959 0.980 0.020
#&gt; SRR1617419     1  0.1414      0.959 0.980 0.020
#&gt; SRR1617420     1  0.0000      0.960 1.000 0.000
#&gt; SRR1617421     1  0.0000      0.960 1.000 0.000
#&gt; SRR1617422     2  0.5629      0.850 0.132 0.868
#&gt; SRR1617423     2  0.5408      0.858 0.124 0.876
#&gt; SRR1617424     1  0.0376      0.960 0.996 0.004
#&gt; SRR1617425     1  0.0376      0.960 0.996 0.004
#&gt; SRR1617427     1  0.0000      0.960 1.000 0.000
#&gt; SRR1617426     1  0.0000      0.960 1.000 0.000
#&gt; SRR1617428     2  0.9000      0.549 0.316 0.684
#&gt; SRR1617429     2  0.8955      0.560 0.312 0.688
#&gt; SRR1617432     1  0.0672      0.959 0.992 0.008
#&gt; SRR1617433     1  0.0672      0.959 0.992 0.008
#&gt; SRR1617434     1  0.0000      0.960 1.000 0.000
#&gt; SRR1617436     1  0.1414      0.959 0.980 0.020
#&gt; SRR1617435     1  0.0000      0.960 1.000 0.000
#&gt; SRR1617437     1  0.1414      0.959 0.980 0.020
#&gt; SRR1617438     1  0.1414      0.959 0.980 0.020
#&gt; SRR1617439     1  0.1414      0.959 0.980 0.020
#&gt; SRR1617440     2  0.6801      0.790 0.180 0.820
#&gt; SRR1617441     2  0.6343      0.813 0.160 0.840
#&gt; SRR1617443     1  0.1414      0.959 0.980 0.020
#&gt; SRR1617442     1  0.1414      0.959 0.980 0.020
#&gt; SRR1617444     2  0.0672      0.938 0.008 0.992
#&gt; SRR1617445     2  0.0672      0.938 0.008 0.992
#&gt; SRR1617446     1  0.0000      0.960 1.000 0.000
#&gt; SRR1617447     1  0.0376      0.960 0.996 0.004
#&gt; SRR1617448     1  0.7674      0.733 0.776 0.224
#&gt; SRR1617449     1  0.7745      0.727 0.772 0.228
#&gt; SRR1617451     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617450     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617452     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617454     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617453     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617456     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617457     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617455     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617458     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617459     2  0.0000      0.939 0.000 1.000
#&gt; SRR1617460     2  0.1414      0.933 0.020 0.980
#&gt; SRR1617461     2  0.1414      0.933 0.020 0.980
#&gt; SRR1617463     2  0.1414      0.933 0.020 0.980
#&gt; SRR1617462     2  0.1414      0.933 0.020 0.980
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617431     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617410     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617411     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617412     3  0.0000     0.9108 0.000 0.000 1.000
#&gt; SRR1617413     3  0.0000     0.9108 0.000 0.000 1.000
#&gt; SRR1617414     1  0.0237     0.9285 0.996 0.000 0.004
#&gt; SRR1617415     1  0.0237     0.9285 0.996 0.000 0.004
#&gt; SRR1617416     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617417     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617418     3  0.0000     0.9108 0.000 0.000 1.000
#&gt; SRR1617419     3  0.0000     0.9108 0.000 0.000 1.000
#&gt; SRR1617420     1  0.4452     0.7706 0.808 0.000 0.192
#&gt; SRR1617421     1  0.4346     0.7802 0.816 0.000 0.184
#&gt; SRR1617422     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617423     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617424     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617425     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617427     1  0.3482     0.8373 0.872 0.000 0.128
#&gt; SRR1617426     1  0.3482     0.8373 0.872 0.000 0.128
#&gt; SRR1617428     1  0.7919     0.3386 0.556 0.380 0.064
#&gt; SRR1617429     1  0.7919     0.3386 0.556 0.380 0.064
#&gt; SRR1617432     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617433     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617434     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617436     3  0.0000     0.9108 0.000 0.000 1.000
#&gt; SRR1617435     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617437     3  0.0000     0.9108 0.000 0.000 1.000
#&gt; SRR1617438     3  0.0000     0.9108 0.000 0.000 1.000
#&gt; SRR1617439     3  0.0000     0.9108 0.000 0.000 1.000
#&gt; SRR1617440     3  0.6260     0.2507 0.000 0.448 0.552
#&gt; SRR1617441     3  0.6309     0.0892 0.000 0.500 0.500
#&gt; SRR1617443     3  0.0000     0.9108 0.000 0.000 1.000
#&gt; SRR1617442     3  0.0000     0.9108 0.000 0.000 1.000
#&gt; SRR1617444     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617445     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617446     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617447     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617448     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617449     1  0.0000     0.9305 1.000 0.000 0.000
#&gt; SRR1617451     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617450     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617452     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617454     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617453     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617456     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617457     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617455     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617458     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617459     2  0.0000     0.9781 0.000 1.000 0.000
#&gt; SRR1617460     2  0.3038     0.8786 0.104 0.896 0.000
#&gt; SRR1617461     2  0.3038     0.8786 0.104 0.896 0.000
#&gt; SRR1617463     2  0.1643     0.9439 0.044 0.956 0.000
#&gt; SRR1617462     2  0.1643     0.9439 0.044 0.956 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     4  0.4967      0.535 0.000 0.452 0.000 0.548
#&gt; SRR1617431     4  0.4989      0.501 0.000 0.472 0.000 0.528
#&gt; SRR1617410     1  0.0000      0.748 1.000 0.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.748 1.000 0.000 0.000 0.000
#&gt; SRR1617412     3  0.0469      0.940 0.000 0.000 0.988 0.012
#&gt; SRR1617413     3  0.0469      0.940 0.000 0.000 0.988 0.012
#&gt; SRR1617414     1  0.1174      0.733 0.968 0.000 0.012 0.020
#&gt; SRR1617415     1  0.1174      0.733 0.968 0.000 0.012 0.020
#&gt; SRR1617416     1  0.4888      0.752 0.588 0.000 0.000 0.412
#&gt; SRR1617417     1  0.4888      0.752 0.588 0.000 0.000 0.412
#&gt; SRR1617418     3  0.0592      0.938 0.000 0.000 0.984 0.016
#&gt; SRR1617419     3  0.0592      0.938 0.000 0.000 0.984 0.016
#&gt; SRR1617420     1  0.2466      0.712 0.900 0.000 0.096 0.004
#&gt; SRR1617421     1  0.2466      0.712 0.900 0.000 0.096 0.004
#&gt; SRR1617422     1  0.4776      0.762 0.624 0.000 0.000 0.376
#&gt; SRR1617423     1  0.4790      0.761 0.620 0.000 0.000 0.380
#&gt; SRR1617424     1  0.4866      0.756 0.596 0.000 0.000 0.404
#&gt; SRR1617425     1  0.4866      0.756 0.596 0.000 0.000 0.404
#&gt; SRR1617427     1  0.4706      0.766 0.748 0.000 0.028 0.224
#&gt; SRR1617426     1  0.4808      0.766 0.736 0.000 0.028 0.236
#&gt; SRR1617428     4  0.5784      0.683 0.112 0.116 0.024 0.748
#&gt; SRR1617429     4  0.5880      0.679 0.116 0.112 0.028 0.744
#&gt; SRR1617432     1  0.0000      0.748 1.000 0.000 0.000 0.000
#&gt; SRR1617433     1  0.0000      0.748 1.000 0.000 0.000 0.000
#&gt; SRR1617434     1  0.0188      0.747 0.996 0.000 0.004 0.000
#&gt; SRR1617436     3  0.0336      0.940 0.000 0.000 0.992 0.008
#&gt; SRR1617435     1  0.0188      0.747 0.996 0.000 0.004 0.000
#&gt; SRR1617437     3  0.0336      0.940 0.000 0.000 0.992 0.008
#&gt; SRR1617438     3  0.0188      0.941 0.000 0.000 0.996 0.004
#&gt; SRR1617439     3  0.0188      0.941 0.000 0.000 0.996 0.004
#&gt; SRR1617440     3  0.4284      0.704 0.000 0.200 0.780 0.020
#&gt; SRR1617441     3  0.4399      0.684 0.000 0.212 0.768 0.020
#&gt; SRR1617443     3  0.0188      0.941 0.000 0.000 0.996 0.004
#&gt; SRR1617442     3  0.0188      0.941 0.000 0.000 0.996 0.004
#&gt; SRR1617444     2  0.2704      0.815 0.000 0.876 0.000 0.124
#&gt; SRR1617445     2  0.2760      0.808 0.000 0.872 0.000 0.128
#&gt; SRR1617446     1  0.5060      0.753 0.584 0.000 0.004 0.412
#&gt; SRR1617447     1  0.5060      0.753 0.584 0.000 0.004 0.412
#&gt; SRR1617448     1  0.5080      0.748 0.576 0.000 0.004 0.420
#&gt; SRR1617449     1  0.5080      0.748 0.576 0.000 0.004 0.420
#&gt; SRR1617451     2  0.0336      0.937 0.000 0.992 0.000 0.008
#&gt; SRR1617450     2  0.0336      0.937 0.000 0.992 0.000 0.008
#&gt; SRR1617452     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; SRR1617454     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; SRR1617453     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; SRR1617456     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; SRR1617457     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; SRR1617455     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; SRR1617458     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; SRR1617459     2  0.0000      0.942 0.000 1.000 0.000 0.000
#&gt; SRR1617460     2  0.2197      0.879 0.080 0.916 0.000 0.004
#&gt; SRR1617461     2  0.2197      0.879 0.080 0.916 0.000 0.004
#&gt; SRR1617463     2  0.1743      0.904 0.056 0.940 0.000 0.004
#&gt; SRR1617462     2  0.1743      0.904 0.056 0.940 0.000 0.004
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     4  0.1768      0.950 0.004 0.072 0.000 0.924 0.000
#&gt; SRR1617431     4  0.1952      0.942 0.004 0.084 0.000 0.912 0.000
#&gt; SRR1617410     5  0.0865      0.969 0.024 0.000 0.000 0.004 0.972
#&gt; SRR1617411     5  0.0865      0.969 0.024 0.000 0.000 0.004 0.972
#&gt; SRR1617412     3  0.1686      0.922 0.028 0.000 0.944 0.020 0.008
#&gt; SRR1617413     3  0.1686      0.922 0.028 0.000 0.944 0.020 0.008
#&gt; SRR1617414     5  0.0912      0.965 0.012 0.000 0.016 0.000 0.972
#&gt; SRR1617415     5  0.0912      0.965 0.012 0.000 0.016 0.000 0.972
#&gt; SRR1617416     1  0.2179      0.891 0.888 0.000 0.000 0.000 0.112
#&gt; SRR1617417     1  0.2179      0.891 0.888 0.000 0.000 0.000 0.112
#&gt; SRR1617418     3  0.1469      0.922 0.036 0.000 0.948 0.016 0.000
#&gt; SRR1617419     3  0.1469      0.922 0.036 0.000 0.948 0.016 0.000
#&gt; SRR1617420     5  0.1410      0.930 0.000 0.000 0.060 0.000 0.940
#&gt; SRR1617421     5  0.1410      0.930 0.000 0.000 0.060 0.000 0.940
#&gt; SRR1617422     1  0.2629      0.880 0.860 0.000 0.000 0.004 0.136
#&gt; SRR1617423     1  0.2753      0.879 0.856 0.000 0.000 0.008 0.136
#&gt; SRR1617424     1  0.2127      0.892 0.892 0.000 0.000 0.000 0.108
#&gt; SRR1617425     1  0.2127      0.892 0.892 0.000 0.000 0.000 0.108
#&gt; SRR1617427     1  0.4560      0.296 0.508 0.000 0.000 0.008 0.484
#&gt; SRR1617426     1  0.4557      0.320 0.516 0.000 0.000 0.008 0.476
#&gt; SRR1617428     4  0.1012      0.951 0.000 0.012 0.000 0.968 0.020
#&gt; SRR1617429     4  0.1012      0.951 0.000 0.012 0.000 0.968 0.020
#&gt; SRR1617432     5  0.0609      0.972 0.020 0.000 0.000 0.000 0.980
#&gt; SRR1617433     5  0.0609      0.972 0.020 0.000 0.000 0.000 0.980
#&gt; SRR1617434     5  0.0609      0.972 0.020 0.000 0.000 0.000 0.980
#&gt; SRR1617436     3  0.0865      0.924 0.000 0.000 0.972 0.024 0.004
#&gt; SRR1617435     5  0.0609      0.972 0.020 0.000 0.000 0.000 0.980
#&gt; SRR1617437     3  0.0955      0.922 0.000 0.000 0.968 0.028 0.004
#&gt; SRR1617438     3  0.0162      0.929 0.000 0.000 0.996 0.004 0.000
#&gt; SRR1617439     3  0.0162      0.929 0.000 0.000 0.996 0.004 0.000
#&gt; SRR1617440     3  0.4681      0.745 0.076 0.148 0.760 0.016 0.000
#&gt; SRR1617441     3  0.4761      0.734 0.076 0.156 0.752 0.016 0.000
#&gt; SRR1617443     3  0.0290      0.928 0.000 0.000 0.992 0.000 0.008
#&gt; SRR1617442     3  0.0290      0.928 0.000 0.000 0.992 0.000 0.008
#&gt; SRR1617444     2  0.4540      0.623 0.300 0.676 0.016 0.008 0.000
#&gt; SRR1617445     2  0.4560      0.617 0.304 0.672 0.016 0.008 0.000
#&gt; SRR1617446     1  0.1892      0.883 0.916 0.000 0.004 0.000 0.080
#&gt; SRR1617447     1  0.1892      0.883 0.916 0.000 0.004 0.000 0.080
#&gt; SRR1617448     1  0.1894      0.875 0.920 0.000 0.008 0.000 0.072
#&gt; SRR1617449     1  0.1768      0.878 0.924 0.000 0.004 0.000 0.072
#&gt; SRR1617451     2  0.1544      0.884 0.000 0.932 0.000 0.068 0.000
#&gt; SRR1617450     2  0.1544      0.884 0.000 0.932 0.000 0.068 0.000
#&gt; SRR1617452     2  0.0000      0.920 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617454     2  0.0162      0.920 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1617453     2  0.0000      0.920 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617456     2  0.0566      0.917 0.004 0.984 0.000 0.012 0.000
#&gt; SRR1617457     2  0.0566      0.917 0.004 0.984 0.000 0.012 0.000
#&gt; SRR1617455     2  0.0162      0.920 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1617458     2  0.0324      0.919 0.004 0.992 0.000 0.004 0.000
#&gt; SRR1617459     2  0.0324      0.919 0.004 0.992 0.000 0.004 0.000
#&gt; SRR1617460     2  0.2173      0.893 0.016 0.920 0.000 0.012 0.052
#&gt; SRR1617461     2  0.2173      0.893 0.016 0.920 0.000 0.012 0.052
#&gt; SRR1617463     2  0.1764      0.904 0.012 0.940 0.000 0.012 0.036
#&gt; SRR1617462     2  0.1764      0.904 0.012 0.940 0.000 0.012 0.036
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     4  0.1408      0.956 0.000 0.020 0.000 0.944 0.000 0.036
#&gt; SRR1617431     4  0.1480      0.951 0.000 0.020 0.000 0.940 0.000 0.040
#&gt; SRR1617410     5  0.0291      0.991 0.000 0.004 0.000 0.004 0.992 0.000
#&gt; SRR1617411     5  0.0291      0.991 0.000 0.004 0.000 0.004 0.992 0.000
#&gt; SRR1617412     3  0.2558      0.715 0.004 0.156 0.840 0.000 0.000 0.000
#&gt; SRR1617413     3  0.2558      0.715 0.004 0.156 0.840 0.000 0.000 0.000
#&gt; SRR1617414     5  0.0260      0.992 0.000 0.008 0.000 0.000 0.992 0.000
#&gt; SRR1617415     5  0.0260      0.992 0.000 0.008 0.000 0.000 0.992 0.000
#&gt; SRR1617416     1  0.2121      0.793 0.892 0.096 0.000 0.012 0.000 0.000
#&gt; SRR1617417     1  0.2121      0.793 0.892 0.096 0.000 0.012 0.000 0.000
#&gt; SRR1617418     3  0.2178      0.731 0.000 0.132 0.868 0.000 0.000 0.000
#&gt; SRR1617419     3  0.2178      0.731 0.000 0.132 0.868 0.000 0.000 0.000
#&gt; SRR1617420     5  0.0405      0.988 0.000 0.000 0.008 0.004 0.988 0.000
#&gt; SRR1617421     5  0.0405      0.988 0.000 0.000 0.008 0.004 0.988 0.000
#&gt; SRR1617422     1  0.1959      0.821 0.924 0.032 0.000 0.000 0.024 0.020
#&gt; SRR1617423     1  0.2201      0.817 0.912 0.032 0.000 0.000 0.028 0.028
#&gt; SRR1617424     1  0.0260      0.833 0.992 0.008 0.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0260      0.833 0.992 0.008 0.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.5083      0.614 0.668 0.084 0.028 0.000 0.220 0.000
#&gt; SRR1617426     1  0.5006      0.626 0.680 0.084 0.028 0.000 0.208 0.000
#&gt; SRR1617428     4  0.0436      0.959 0.000 0.000 0.004 0.988 0.004 0.004
#&gt; SRR1617429     4  0.0436      0.959 0.000 0.000 0.004 0.988 0.004 0.004
#&gt; SRR1617432     5  0.0363      0.991 0.000 0.012 0.000 0.000 0.988 0.000
#&gt; SRR1617433     5  0.0363      0.991 0.000 0.012 0.000 0.000 0.988 0.000
#&gt; SRR1617434     5  0.0000      0.993 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617436     3  0.2790      0.782 0.000 0.132 0.844 0.024 0.000 0.000
#&gt; SRR1617435     5  0.0000      0.993 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617437     3  0.2790      0.782 0.000 0.132 0.844 0.024 0.000 0.000
#&gt; SRR1617438     3  0.2664      0.749 0.000 0.184 0.816 0.000 0.000 0.000
#&gt; SRR1617439     3  0.2664      0.749 0.000 0.184 0.816 0.000 0.000 0.000
#&gt; SRR1617440     2  0.4301      0.972 0.000 0.696 0.240 0.000 0.000 0.064
#&gt; SRR1617441     2  0.4455      0.972 0.000 0.684 0.240 0.000 0.000 0.076
#&gt; SRR1617443     3  0.2431      0.785 0.000 0.132 0.860 0.000 0.008 0.000
#&gt; SRR1617442     3  0.2431      0.785 0.000 0.132 0.860 0.000 0.008 0.000
#&gt; SRR1617444     1  0.5944      0.213 0.476 0.380 0.024 0.000 0.000 0.120
#&gt; SRR1617445     1  0.5964      0.224 0.480 0.372 0.024 0.000 0.000 0.124
#&gt; SRR1617446     1  0.0146      0.834 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; SRR1617447     1  0.0146      0.834 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; SRR1617448     1  0.0713      0.833 0.972 0.028 0.000 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0713      0.833 0.972 0.028 0.000 0.000 0.000 0.000
#&gt; SRR1617451     6  0.5563      0.507 0.000 0.260 0.000 0.192 0.000 0.548
#&gt; SRR1617450     6  0.5488      0.523 0.000 0.272 0.000 0.172 0.000 0.556
#&gt; SRR1617452     6  0.1556      0.759 0.000 0.080 0.000 0.000 0.000 0.920
#&gt; SRR1617454     6  0.1327      0.758 0.000 0.064 0.000 0.000 0.000 0.936
#&gt; SRR1617453     6  0.1556      0.759 0.000 0.080 0.000 0.000 0.000 0.920
#&gt; SRR1617456     6  0.3531      0.659 0.000 0.328 0.000 0.000 0.000 0.672
#&gt; SRR1617457     6  0.3531      0.659 0.000 0.328 0.000 0.000 0.000 0.672
#&gt; SRR1617455     6  0.1141      0.758 0.000 0.052 0.000 0.000 0.000 0.948
#&gt; SRR1617458     6  0.3428      0.675 0.000 0.304 0.000 0.000 0.000 0.696
#&gt; SRR1617459     6  0.3390      0.680 0.000 0.296 0.000 0.000 0.000 0.704
#&gt; SRR1617460     6  0.2618      0.710 0.036 0.060 0.000 0.004 0.012 0.888
#&gt; SRR1617461     6  0.2618      0.710 0.036 0.060 0.000 0.004 0.012 0.888
#&gt; SRR1617463     6  0.2063      0.726 0.020 0.060 0.000 0.000 0.008 0.912
#&gt; SRR1617462     6  0.2063      0.726 0.020 0.060 0.000 0.000 0.008 0.912
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.602           0.703       0.891         0.4446 0.525   0.525
#> 3 3 0.517           0.487       0.712         0.3416 0.883   0.776
#> 4 4 0.638           0.650       0.857         0.1230 0.888   0.731
#> 5 5 0.614           0.762       0.784         0.1119 0.857   0.588
#> 6 6 0.673           0.648       0.784         0.0791 0.950   0.788
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617431     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617410     1   0.996    -0.0607 0.536 0.464
#&gt; SRR1617411     1   0.996    -0.0607 0.536 0.464
#&gt; SRR1617412     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617413     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617414     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617415     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617416     2   0.000     0.7953 0.000 1.000
#&gt; SRR1617417     2   0.000     0.7953 0.000 1.000
#&gt; SRR1617418     2   0.388     0.7732 0.076 0.924
#&gt; SRR1617419     2   0.388     0.7732 0.076 0.924
#&gt; SRR1617420     1   0.260     0.8581 0.956 0.044
#&gt; SRR1617421     1   0.260     0.8581 0.956 0.044
#&gt; SRR1617422     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617423     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617424     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617425     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617427     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617426     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617428     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617429     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617432     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617433     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617434     2   0.000     0.7953 0.000 1.000
#&gt; SRR1617436     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617435     2   0.000     0.7953 0.000 1.000
#&gt; SRR1617437     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617438     1   0.980     0.1589 0.584 0.416
#&gt; SRR1617439     1   0.980     0.1589 0.584 0.416
#&gt; SRR1617440     2   0.999     0.1794 0.484 0.516
#&gt; SRR1617441     2   0.999     0.1794 0.484 0.516
#&gt; SRR1617443     2   0.000     0.7953 0.000 1.000
#&gt; SRR1617442     2   0.000     0.7953 0.000 1.000
#&gt; SRR1617444     2   0.998     0.2017 0.476 0.524
#&gt; SRR1617445     2   0.998     0.2017 0.476 0.524
#&gt; SRR1617446     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617447     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617448     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617449     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617451     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617450     1   0.000     0.8931 1.000 0.000
#&gt; SRR1617452     2   0.000     0.7953 0.000 1.000
#&gt; SRR1617454     1   0.506     0.7900 0.888 0.112
#&gt; SRR1617453     2   0.000     0.7953 0.000 1.000
#&gt; SRR1617456     2   0.204     0.7953 0.032 0.968
#&gt; SRR1617457     2   0.204     0.7953 0.032 0.968
#&gt; SRR1617455     1   0.506     0.7900 0.888 0.112
#&gt; SRR1617458     2   0.204     0.7953 0.032 0.968
#&gt; SRR1617459     2   0.204     0.7953 0.032 0.968
#&gt; SRR1617460     2   1.000     0.1531 0.492 0.508
#&gt; SRR1617461     2   1.000     0.1531 0.492 0.508
#&gt; SRR1617463     1   0.904     0.4329 0.680 0.320
#&gt; SRR1617462     1   0.904     0.4329 0.680 0.320
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2   0.630      1.000 0.480 0.520 0.000
#&gt; SRR1617431     2   0.630      1.000 0.480 0.520 0.000
#&gt; SRR1617410     1   0.630      0.224 0.528 0.000 0.472
#&gt; SRR1617411     1   0.630      0.224 0.528 0.000 0.472
#&gt; SRR1617412     1   0.000      0.680 1.000 0.000 0.000
#&gt; SRR1617413     1   0.000      0.680 1.000 0.000 0.000
#&gt; SRR1617414     1   0.000      0.680 1.000 0.000 0.000
#&gt; SRR1617415     1   0.000      0.680 1.000 0.000 0.000
#&gt; SRR1617416     3   0.630      0.601 0.000 0.480 0.520
#&gt; SRR1617417     3   0.630      0.601 0.000 0.480 0.520
#&gt; SRR1617418     3   0.226      0.459 0.068 0.000 0.932
#&gt; SRR1617419     3   0.226      0.459 0.068 0.000 0.932
#&gt; SRR1617420     1   0.164      0.665 0.956 0.000 0.044
#&gt; SRR1617421     1   0.164      0.665 0.956 0.000 0.044
#&gt; SRR1617422     1   0.000      0.680 1.000 0.000 0.000
#&gt; SRR1617423     1   0.000      0.680 1.000 0.000 0.000
#&gt; SRR1617424     1   0.000      0.680 1.000 0.000 0.000
#&gt; SRR1617425     1   0.000      0.680 1.000 0.000 0.000
#&gt; SRR1617427     1   0.622     -0.784 0.568 0.432 0.000
#&gt; SRR1617426     1   0.622     -0.784 0.568 0.432 0.000
#&gt; SRR1617428     2   0.630      1.000 0.480 0.520 0.000
#&gt; SRR1617429     2   0.630      1.000 0.480 0.520 0.000
#&gt; SRR1617432     1   0.000      0.680 1.000 0.000 0.000
#&gt; SRR1617433     1   0.000      0.680 1.000 0.000 0.000
#&gt; SRR1617434     3   0.618      0.611 0.000 0.416 0.584
#&gt; SRR1617436     1   0.334      0.507 0.880 0.120 0.000
#&gt; SRR1617435     3   0.618      0.611 0.000 0.416 0.584
#&gt; SRR1617437     1   0.334      0.507 0.880 0.120 0.000
#&gt; SRR1617438     1   0.620      0.342 0.576 0.000 0.424
#&gt; SRR1617439     1   0.620      0.342 0.576 0.000 0.424
#&gt; SRR1617440     3   0.630     -0.177 0.476 0.000 0.524
#&gt; SRR1617441     3   0.630     -0.177 0.476 0.000 0.524
#&gt; SRR1617443     3   0.618      0.611 0.000 0.416 0.584
#&gt; SRR1617442     3   0.618      0.611 0.000 0.416 0.584
#&gt; SRR1617444     3   0.629     -0.162 0.468 0.000 0.532
#&gt; SRR1617445     3   0.629     -0.162 0.468 0.000 0.532
#&gt; SRR1617446     1   0.288      0.560 0.904 0.096 0.000
#&gt; SRR1617447     1   0.288      0.560 0.904 0.096 0.000
#&gt; SRR1617448     1   0.288      0.560 0.904 0.096 0.000
#&gt; SRR1617449     1   0.288      0.560 0.904 0.096 0.000
#&gt; SRR1617451     2   0.630      1.000 0.480 0.520 0.000
#&gt; SRR1617450     2   0.630      1.000 0.480 0.520 0.000
#&gt; SRR1617452     3   0.630      0.601 0.000 0.480 0.520
#&gt; SRR1617454     1   0.447      0.614 0.852 0.028 0.120
#&gt; SRR1617453     3   0.630      0.601 0.000 0.480 0.520
#&gt; SRR1617456     3   0.730      0.611 0.032 0.412 0.556
#&gt; SRR1617457     3   0.730      0.611 0.032 0.412 0.556
#&gt; SRR1617455     1   0.447      0.614 0.852 0.028 0.120
#&gt; SRR1617458     3   0.730      0.611 0.032 0.412 0.556
#&gt; SRR1617459     3   0.730      0.611 0.032 0.412 0.556
#&gt; SRR1617460     3   0.630     -0.194 0.484 0.000 0.516
#&gt; SRR1617461     3   0.630     -0.194 0.484 0.000 0.516
#&gt; SRR1617463     1   0.576      0.491 0.672 0.000 0.328
#&gt; SRR1617462     1   0.576      0.491 0.672 0.000 0.328
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2   0.000     0.8984 0.000 1.000 0.000 0.000
#&gt; SRR1617431     2   0.000     0.8984 0.000 1.000 0.000 0.000
#&gt; SRR1617410     1   0.500    -0.5520 0.516 0.000 0.484 0.000
#&gt; SRR1617411     1   0.500    -0.5520 0.516 0.000 0.484 0.000
#&gt; SRR1617412     1   0.000     0.7563 1.000 0.000 0.000 0.000
#&gt; SRR1617413     1   0.000     0.7563 1.000 0.000 0.000 0.000
#&gt; SRR1617414     1   0.000     0.7563 1.000 0.000 0.000 0.000
#&gt; SRR1617415     1   0.000     0.7563 1.000 0.000 0.000 0.000
#&gt; SRR1617416     4   0.000     0.9200 0.000 0.000 0.000 1.000
#&gt; SRR1617417     4   0.000     0.9200 0.000 0.000 0.000 1.000
#&gt; SRR1617418     3   0.000     0.3895 0.000 0.000 1.000 0.000
#&gt; SRR1617419     3   0.000     0.3895 0.000 0.000 1.000 0.000
#&gt; SRR1617420     1   0.130     0.7267 0.956 0.000 0.044 0.000
#&gt; SRR1617421     1   0.130     0.7267 0.956 0.000 0.044 0.000
#&gt; SRR1617422     1   0.000     0.7563 1.000 0.000 0.000 0.000
#&gt; SRR1617423     1   0.000     0.7563 1.000 0.000 0.000 0.000
#&gt; SRR1617424     1   0.000     0.7563 1.000 0.000 0.000 0.000
#&gt; SRR1617425     1   0.000     0.7563 1.000 0.000 0.000 0.000
#&gt; SRR1617427     2   0.407     0.7137 0.252 0.748 0.000 0.000
#&gt; SRR1617426     2   0.407     0.7137 0.252 0.748 0.000 0.000
#&gt; SRR1617428     2   0.102     0.8966 0.032 0.968 0.000 0.000
#&gt; SRR1617429     2   0.102     0.8966 0.032 0.968 0.000 0.000
#&gt; SRR1617432     1   0.000     0.7563 1.000 0.000 0.000 0.000
#&gt; SRR1617433     1   0.000     0.7563 1.000 0.000 0.000 0.000
#&gt; SRR1617434     4   0.234     0.9046 0.000 0.000 0.100 0.900
#&gt; SRR1617436     1   0.281     0.6731 0.868 0.132 0.000 0.000
#&gt; SRR1617435     4   0.234     0.9046 0.000 0.000 0.100 0.900
#&gt; SRR1617437     1   0.281     0.6731 0.868 0.132 0.000 0.000
#&gt; SRR1617438     1   0.496    -0.3925 0.552 0.000 0.448 0.000
#&gt; SRR1617439     1   0.496    -0.3925 0.552 0.000 0.448 0.000
#&gt; SRR1617440     3   0.495     0.6909 0.440 0.000 0.560 0.000
#&gt; SRR1617441     3   0.495     0.6909 0.440 0.000 0.560 0.000
#&gt; SRR1617443     4   0.234     0.9046 0.000 0.000 0.100 0.900
#&gt; SRR1617442     4   0.234     0.9046 0.000 0.000 0.100 0.900
#&gt; SRR1617444     3   0.524     0.6937 0.432 0.000 0.560 0.008
#&gt; SRR1617445     3   0.524     0.6937 0.432 0.000 0.560 0.008
#&gt; SRR1617446     1   0.228     0.7063 0.904 0.096 0.000 0.000
#&gt; SRR1617447     1   0.228     0.7063 0.904 0.096 0.000 0.000
#&gt; SRR1617448     1   0.228     0.7063 0.904 0.096 0.000 0.000
#&gt; SRR1617449     1   0.228     0.7063 0.904 0.096 0.000 0.000
#&gt; SRR1617451     2   0.000     0.8984 0.000 1.000 0.000 0.000
#&gt; SRR1617450     2   0.000     0.8984 0.000 1.000 0.000 0.000
#&gt; SRR1617452     4   0.000     0.9200 0.000 0.000 0.000 1.000
#&gt; SRR1617454     1   0.410     0.5807 0.808 0.028 0.164 0.000
#&gt; SRR1617453     4   0.000     0.9200 0.000 0.000 0.000 1.000
#&gt; SRR1617456     4   0.234     0.9056 0.000 0.000 0.100 0.900
#&gt; SRR1617457     4   0.234     0.9056 0.000 0.000 0.100 0.900
#&gt; SRR1617455     1   0.410     0.5807 0.808 0.028 0.164 0.000
#&gt; SRR1617458     4   0.234     0.9056 0.000 0.000 0.100 0.900
#&gt; SRR1617459     4   0.234     0.9056 0.000 0.000 0.100 0.900
#&gt; SRR1617460     3   0.498     0.6593 0.460 0.000 0.540 0.000
#&gt; SRR1617461     3   0.498     0.6593 0.460 0.000 0.540 0.000
#&gt; SRR1617463     1   0.462     0.0301 0.660 0.000 0.340 0.000
#&gt; SRR1617462     1   0.462     0.0301 0.660 0.000 0.340 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     4  0.0000      0.837 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617431     4  0.0000      0.837 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617410     2  0.2424      0.715 0.132 0.868 0.000 0.000 0.000
#&gt; SRR1617411     2  0.2424      0.715 0.132 0.868 0.000 0.000 0.000
#&gt; SRR1617412     1  0.3861      0.738 0.804 0.128 0.068 0.000 0.000
#&gt; SRR1617413     1  0.3861      0.738 0.804 0.128 0.068 0.000 0.000
#&gt; SRR1617414     1  0.5426      0.750 0.640 0.252 0.108 0.000 0.000
#&gt; SRR1617415     1  0.5426      0.750 0.640 0.252 0.108 0.000 0.000
#&gt; SRR1617416     5  0.0162      0.907 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1617417     5  0.0162      0.907 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1617418     3  0.3242      1.000 0.000 0.216 0.784 0.000 0.000
#&gt; SRR1617419     3  0.3242      1.000 0.000 0.216 0.784 0.000 0.000
#&gt; SRR1617420     1  0.4687      0.722 0.672 0.288 0.040 0.000 0.000
#&gt; SRR1617421     1  0.4687      0.722 0.672 0.288 0.040 0.000 0.000
#&gt; SRR1617422     1  0.4883      0.703 0.652 0.300 0.048 0.000 0.000
#&gt; SRR1617423     1  0.4883      0.703 0.652 0.300 0.048 0.000 0.000
#&gt; SRR1617424     1  0.4883      0.703 0.652 0.300 0.048 0.000 0.000
#&gt; SRR1617425     1  0.4883      0.703 0.652 0.300 0.048 0.000 0.000
#&gt; SRR1617427     4  0.4101      0.569 0.372 0.000 0.000 0.628 0.000
#&gt; SRR1617426     4  0.4101      0.569 0.372 0.000 0.000 0.628 0.000
#&gt; SRR1617428     4  0.1341      0.831 0.056 0.000 0.000 0.944 0.000
#&gt; SRR1617429     4  0.1341      0.831 0.056 0.000 0.000 0.944 0.000
#&gt; SRR1617432     1  0.5426      0.750 0.640 0.252 0.108 0.000 0.000
#&gt; SRR1617433     1  0.5426      0.750 0.640 0.252 0.108 0.000 0.000
#&gt; SRR1617434     5  0.2020      0.877 0.000 0.100 0.000 0.000 0.900
#&gt; SRR1617436     1  0.1952      0.646 0.912 0.000 0.084 0.004 0.000
#&gt; SRR1617435     5  0.2020      0.877 0.000 0.100 0.000 0.000 0.900
#&gt; SRR1617437     1  0.1952      0.646 0.912 0.000 0.084 0.004 0.000
#&gt; SRR1617438     2  0.2770      0.767 0.076 0.880 0.044 0.000 0.000
#&gt; SRR1617439     2  0.2770      0.767 0.076 0.880 0.044 0.000 0.000
#&gt; SRR1617440     2  0.0324      0.777 0.004 0.992 0.004 0.000 0.000
#&gt; SRR1617441     2  0.0324      0.777 0.004 0.992 0.004 0.000 0.000
#&gt; SRR1617443     5  0.2020      0.877 0.000 0.100 0.000 0.000 0.900
#&gt; SRR1617442     5  0.2020      0.877 0.000 0.100 0.000 0.000 0.900
#&gt; SRR1617444     2  0.0613      0.772 0.004 0.984 0.004 0.000 0.008
#&gt; SRR1617445     2  0.0613      0.772 0.004 0.984 0.004 0.000 0.008
#&gt; SRR1617446     1  0.3033      0.748 0.864 0.084 0.052 0.000 0.000
#&gt; SRR1617447     1  0.3033      0.748 0.864 0.084 0.052 0.000 0.000
#&gt; SRR1617448     1  0.3033      0.748 0.864 0.084 0.052 0.000 0.000
#&gt; SRR1617449     1  0.3033      0.748 0.864 0.084 0.052 0.000 0.000
#&gt; SRR1617451     4  0.0000      0.837 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617450     4  0.0000      0.837 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1617452     5  0.0162      0.907 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1617454     2  0.6451      0.129 0.372 0.504 0.096 0.028 0.000
#&gt; SRR1617453     5  0.0162      0.907 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1617456     5  0.2450      0.887 0.000 0.052 0.048 0.000 0.900
#&gt; SRR1617457     5  0.2450      0.887 0.000 0.052 0.048 0.000 0.900
#&gt; SRR1617455     2  0.6451      0.129 0.372 0.504 0.096 0.028 0.000
#&gt; SRR1617458     5  0.2450      0.887 0.000 0.052 0.048 0.000 0.900
#&gt; SRR1617459     5  0.2450      0.887 0.000 0.052 0.048 0.000 0.900
#&gt; SRR1617460     2  0.0671      0.784 0.016 0.980 0.004 0.000 0.000
#&gt; SRR1617461     2  0.0671      0.784 0.016 0.980 0.004 0.000 0.000
#&gt; SRR1617463     2  0.4021      0.696 0.168 0.780 0.052 0.000 0.000
#&gt; SRR1617462     2  0.4021      0.696 0.168 0.780 0.052 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.0000      0.854 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617431     2  0.0000      0.854 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1617410     3  0.3309      0.460 0.000 0.000 0.720 0.000 0.280 0.000
#&gt; SRR1617411     3  0.3309      0.460 0.000 0.000 0.720 0.000 0.280 0.000
#&gt; SRR1617412     1  0.3364      0.392 0.780 0.000 0.024 0.000 0.196 0.000
#&gt; SRR1617413     1  0.3364      0.392 0.780 0.000 0.024 0.000 0.196 0.000
#&gt; SRR1617414     5  0.1807      0.752 0.020 0.000 0.060 0.000 0.920 0.000
#&gt; SRR1617415     5  0.1807      0.752 0.020 0.000 0.060 0.000 0.920 0.000
#&gt; SRR1617416     6  0.2178      0.863 0.000 0.000 0.000 0.132 0.000 0.868
#&gt; SRR1617417     6  0.2178      0.863 0.000 0.000 0.000 0.132 0.000 0.868
#&gt; SRR1617418     4  0.2219      1.000 0.000 0.000 0.136 0.864 0.000 0.000
#&gt; SRR1617419     4  0.2219      1.000 0.000 0.000 0.136 0.864 0.000 0.000
#&gt; SRR1617420     5  0.5271      0.283 0.380 0.000 0.104 0.000 0.516 0.000
#&gt; SRR1617421     5  0.5271      0.283 0.380 0.000 0.104 0.000 0.516 0.000
#&gt; SRR1617422     1  0.6075      0.118 0.396 0.000 0.280 0.000 0.324 0.000
#&gt; SRR1617423     1  0.6075      0.118 0.396 0.000 0.280 0.000 0.324 0.000
#&gt; SRR1617424     1  0.6075      0.118 0.396 0.000 0.280 0.000 0.324 0.000
#&gt; SRR1617425     1  0.6075      0.118 0.396 0.000 0.280 0.000 0.324 0.000
#&gt; SRR1617427     2  0.3684      0.612 0.372 0.628 0.000 0.000 0.000 0.000
#&gt; SRR1617426     2  0.3684      0.612 0.372 0.628 0.000 0.000 0.000 0.000
#&gt; SRR1617428     2  0.1204      0.850 0.056 0.944 0.000 0.000 0.000 0.000
#&gt; SRR1617429     2  0.1204      0.850 0.056 0.944 0.000 0.000 0.000 0.000
#&gt; SRR1617432     5  0.1807      0.752 0.020 0.000 0.060 0.000 0.920 0.000
#&gt; SRR1617433     5  0.1807      0.752 0.020 0.000 0.060 0.000 0.920 0.000
#&gt; SRR1617434     6  0.1814      0.866 0.000 0.000 0.100 0.000 0.000 0.900
#&gt; SRR1617436     1  0.0713      0.447 0.972 0.000 0.000 0.000 0.028 0.000
#&gt; SRR1617435     6  0.1814      0.866 0.000 0.000 0.100 0.000 0.000 0.900
#&gt; SRR1617437     1  0.0713      0.447 0.972 0.000 0.000 0.000 0.028 0.000
#&gt; SRR1617438     3  0.2617      0.745 0.100 0.000 0.872 0.016 0.012 0.000
#&gt; SRR1617439     3  0.2617      0.745 0.100 0.000 0.872 0.016 0.012 0.000
#&gt; SRR1617440     3  0.0458      0.760 0.000 0.000 0.984 0.016 0.000 0.000
#&gt; SRR1617441     3  0.0458      0.760 0.000 0.000 0.984 0.016 0.000 0.000
#&gt; SRR1617443     6  0.1814      0.866 0.000 0.000 0.100 0.000 0.000 0.900
#&gt; SRR1617442     6  0.1814      0.866 0.000 0.000 0.100 0.000 0.000 0.900
#&gt; SRR1617444     3  0.0717      0.759 0.000 0.000 0.976 0.016 0.000 0.008
#&gt; SRR1617445     3  0.0717      0.759 0.000 0.000 0.976 0.016 0.000 0.008
#&gt; SRR1617446     1  0.3852      0.409 0.664 0.000 0.012 0.000 0.324 0.000
#&gt; SRR1617447     1  0.3852      0.409 0.664 0.000 0.012 0.000 0.324 0.000
#&gt; SRR1617448     1  0.3852      0.409 0.664 0.000 0.012 0.000 0.324 0.000
#&gt; SRR1617449     1  0.3852      0.409 0.664 0.000 0.012 0.000 0.324 0.000
#&gt; SRR1617451     2  0.0777      0.851 0.000 0.972 0.000 0.004 0.024 0.000
#&gt; SRR1617450     2  0.0777      0.851 0.000 0.972 0.000 0.004 0.024 0.000
#&gt; SRR1617452     6  0.2178      0.863 0.000 0.000 0.000 0.132 0.000 0.868
#&gt; SRR1617454     3  0.5420      0.335 0.104 0.000 0.500 0.004 0.392 0.000
#&gt; SRR1617453     6  0.2178      0.863 0.000 0.000 0.000 0.132 0.000 0.868
#&gt; SRR1617456     6  0.2263      0.846 0.000 0.000 0.048 0.000 0.056 0.896
#&gt; SRR1617457     6  0.2263      0.846 0.000 0.000 0.048 0.000 0.056 0.896
#&gt; SRR1617455     3  0.5420      0.335 0.104 0.000 0.500 0.004 0.392 0.000
#&gt; SRR1617458     6  0.2263      0.846 0.000 0.000 0.048 0.000 0.056 0.896
#&gt; SRR1617459     6  0.2263      0.846 0.000 0.000 0.048 0.000 0.056 0.896
#&gt; SRR1617460     3  0.0146      0.764 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1617461     3  0.0146      0.764 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1617463     3  0.3563      0.686 0.072 0.000 0.796 0.000 0.132 0.000
#&gt; SRR1617462     3  0.3563      0.686 0.072 0.000 0.796 0.000 0.132 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:kmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.925           0.925       0.973         0.4930 0.508   0.508
#> 3 3 0.477           0.700       0.796         0.3011 0.804   0.625
#> 4 4 0.496           0.602       0.699         0.1027 0.855   0.643
#> 5 5 0.496           0.411       0.625         0.0723 0.983   0.949
#> 6 6 0.514           0.317       0.576         0.0453 0.936   0.794
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     1   0.000      0.969 1.000 0.000
#&gt; SRR1617431     1   0.000      0.969 1.000 0.000
#&gt; SRR1617410     2   0.850      0.615 0.276 0.724
#&gt; SRR1617411     2   0.850      0.615 0.276 0.724
#&gt; SRR1617412     1   0.000      0.969 1.000 0.000
#&gt; SRR1617413     1   0.000      0.969 1.000 0.000
#&gt; SRR1617414     1   0.000      0.969 1.000 0.000
#&gt; SRR1617415     1   0.000      0.969 1.000 0.000
#&gt; SRR1617416     2   0.000      0.972 0.000 1.000
#&gt; SRR1617417     2   0.000      0.972 0.000 1.000
#&gt; SRR1617418     2   0.000      0.972 0.000 1.000
#&gt; SRR1617419     2   0.000      0.972 0.000 1.000
#&gt; SRR1617420     1   0.000      0.969 1.000 0.000
#&gt; SRR1617421     1   0.000      0.969 1.000 0.000
#&gt; SRR1617422     1   0.000      0.969 1.000 0.000
#&gt; SRR1617423     1   0.000      0.969 1.000 0.000
#&gt; SRR1617424     1   0.000      0.969 1.000 0.000
#&gt; SRR1617425     1   0.000      0.969 1.000 0.000
#&gt; SRR1617427     1   0.000      0.969 1.000 0.000
#&gt; SRR1617426     1   0.000      0.969 1.000 0.000
#&gt; SRR1617428     1   0.000      0.969 1.000 0.000
#&gt; SRR1617429     1   0.000      0.969 1.000 0.000
#&gt; SRR1617432     1   0.000      0.969 1.000 0.000
#&gt; SRR1617433     1   0.000      0.969 1.000 0.000
#&gt; SRR1617434     2   0.000      0.972 0.000 1.000
#&gt; SRR1617436     1   0.000      0.969 1.000 0.000
#&gt; SRR1617435     2   0.000      0.972 0.000 1.000
#&gt; SRR1617437     1   0.000      0.969 1.000 0.000
#&gt; SRR1617438     1   0.995      0.120 0.540 0.460
#&gt; SRR1617439     1   0.995      0.120 0.540 0.460
#&gt; SRR1617440     2   0.000      0.972 0.000 1.000
#&gt; SRR1617441     2   0.000      0.972 0.000 1.000
#&gt; SRR1617443     2   0.000      0.972 0.000 1.000
#&gt; SRR1617442     2   0.000      0.972 0.000 1.000
#&gt; SRR1617444     2   0.000      0.972 0.000 1.000
#&gt; SRR1617445     2   0.000      0.972 0.000 1.000
#&gt; SRR1617446     1   0.000      0.969 1.000 0.000
#&gt; SRR1617447     1   0.000      0.969 1.000 0.000
#&gt; SRR1617448     1   0.000      0.969 1.000 0.000
#&gt; SRR1617449     1   0.000      0.969 1.000 0.000
#&gt; SRR1617451     1   0.000      0.969 1.000 0.000
#&gt; SRR1617450     1   0.000      0.969 1.000 0.000
#&gt; SRR1617452     2   0.000      0.972 0.000 1.000
#&gt; SRR1617454     1   0.000      0.969 1.000 0.000
#&gt; SRR1617453     2   0.000      0.972 0.000 1.000
#&gt; SRR1617456     2   0.000      0.972 0.000 1.000
#&gt; SRR1617457     2   0.000      0.972 0.000 1.000
#&gt; SRR1617455     1   0.000      0.969 1.000 0.000
#&gt; SRR1617458     2   0.000      0.972 0.000 1.000
#&gt; SRR1617459     2   0.000      0.972 0.000 1.000
#&gt; SRR1617460     2   0.000      0.972 0.000 1.000
#&gt; SRR1617461     2   0.000      0.972 0.000 1.000
#&gt; SRR1617463     1   0.000      0.969 1.000 0.000
#&gt; SRR1617462     1   0.000      0.969 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.0000      0.679 0.000 1.000 0.000
#&gt; SRR1617431     2  0.0000      0.679 0.000 1.000 0.000
#&gt; SRR1617410     1  0.3669      0.765 0.896 0.040 0.064
#&gt; SRR1617411     1  0.3669      0.765 0.896 0.040 0.064
#&gt; SRR1617412     1  0.4842      0.815 0.776 0.224 0.000
#&gt; SRR1617413     1  0.4842      0.815 0.776 0.224 0.000
#&gt; SRR1617414     2  0.4555      0.688 0.200 0.800 0.000
#&gt; SRR1617415     2  0.4555      0.688 0.200 0.800 0.000
#&gt; SRR1617416     3  0.1860      0.851 0.052 0.000 0.948
#&gt; SRR1617417     3  0.1860      0.851 0.052 0.000 0.948
#&gt; SRR1617418     3  0.6244      0.576 0.440 0.000 0.560
#&gt; SRR1617419     3  0.6244      0.576 0.440 0.000 0.560
#&gt; SRR1617420     1  0.4504      0.848 0.804 0.196 0.000
#&gt; SRR1617421     1  0.4504      0.848 0.804 0.196 0.000
#&gt; SRR1617422     2  0.4346      0.686 0.184 0.816 0.000
#&gt; SRR1617423     2  0.4346      0.686 0.184 0.816 0.000
#&gt; SRR1617424     1  0.4605      0.844 0.796 0.204 0.000
#&gt; SRR1617425     1  0.4605      0.844 0.796 0.204 0.000
#&gt; SRR1617427     2  0.2711      0.696 0.088 0.912 0.000
#&gt; SRR1617426     2  0.2711      0.696 0.088 0.912 0.000
#&gt; SRR1617428     2  0.0237      0.677 0.004 0.996 0.000
#&gt; SRR1617429     2  0.0237      0.677 0.004 0.996 0.000
#&gt; SRR1617432     1  0.4504      0.833 0.804 0.196 0.000
#&gt; SRR1617433     1  0.4504      0.833 0.804 0.196 0.000
#&gt; SRR1617434     3  0.2878      0.875 0.096 0.000 0.904
#&gt; SRR1617436     2  0.6252      0.321 0.444 0.556 0.000
#&gt; SRR1617435     3  0.2878      0.875 0.096 0.000 0.904
#&gt; SRR1617437     2  0.6252      0.321 0.444 0.556 0.000
#&gt; SRR1617438     1  0.3481      0.768 0.904 0.044 0.052
#&gt; SRR1617439     1  0.3481      0.768 0.904 0.044 0.052
#&gt; SRR1617440     3  0.5785      0.775 0.332 0.000 0.668
#&gt; SRR1617441     3  0.5785      0.775 0.332 0.000 0.668
#&gt; SRR1617443     3  0.2448      0.873 0.076 0.000 0.924
#&gt; SRR1617442     3  0.2448      0.873 0.076 0.000 0.924
#&gt; SRR1617444     3  0.4796      0.852 0.220 0.000 0.780
#&gt; SRR1617445     3  0.4796      0.852 0.220 0.000 0.780
#&gt; SRR1617446     2  0.6308      0.182 0.492 0.508 0.000
#&gt; SRR1617447     2  0.6308      0.182 0.492 0.508 0.000
#&gt; SRR1617448     2  0.6308      0.182 0.492 0.508 0.000
#&gt; SRR1617449     2  0.6308      0.182 0.492 0.508 0.000
#&gt; SRR1617451     2  0.0000      0.679 0.000 1.000 0.000
#&gt; SRR1617450     2  0.0000      0.679 0.000 1.000 0.000
#&gt; SRR1617452     3  0.1411      0.851 0.036 0.000 0.964
#&gt; SRR1617454     2  0.5988      0.556 0.304 0.688 0.008
#&gt; SRR1617453     3  0.1411      0.851 0.036 0.000 0.964
#&gt; SRR1617456     3  0.2878      0.870 0.096 0.000 0.904
#&gt; SRR1617457     3  0.2878      0.870 0.096 0.000 0.904
#&gt; SRR1617455     2  0.5988      0.556 0.304 0.688 0.008
#&gt; SRR1617458     3  0.2878      0.870 0.096 0.000 0.904
#&gt; SRR1617459     3  0.2878      0.870 0.096 0.000 0.904
#&gt; SRR1617460     3  0.4931      0.844 0.232 0.000 0.768
#&gt; SRR1617461     3  0.4931      0.844 0.232 0.000 0.768
#&gt; SRR1617463     2  0.6483      0.458 0.392 0.600 0.008
#&gt; SRR1617462     2  0.6483      0.458 0.392 0.600 0.008
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2   0.260     0.7236 0.068 0.908 0.000 0.024
#&gt; SRR1617431     2   0.260     0.7236 0.068 0.908 0.000 0.024
#&gt; SRR1617410     1   0.536     0.5587 0.744 0.000 0.108 0.148
#&gt; SRR1617411     1   0.536     0.5587 0.744 0.000 0.108 0.148
#&gt; SRR1617412     1   0.328     0.6568 0.872 0.032 0.000 0.096
#&gt; SRR1617413     1   0.328     0.6568 0.872 0.032 0.000 0.096
#&gt; SRR1617414     2   0.684     0.5830 0.280 0.580 0.000 0.140
#&gt; SRR1617415     2   0.684     0.5830 0.280 0.580 0.000 0.140
#&gt; SRR1617416     3   0.438     0.7186 0.000 0.036 0.792 0.172
#&gt; SRR1617417     3   0.438     0.7186 0.000 0.036 0.792 0.172
#&gt; SRR1617418     3   0.814     0.4133 0.280 0.016 0.452 0.252
#&gt; SRR1617419     3   0.814     0.4133 0.280 0.016 0.452 0.252
#&gt; SRR1617420     1   0.152     0.6669 0.956 0.020 0.000 0.024
#&gt; SRR1617421     1   0.152     0.6669 0.956 0.020 0.000 0.024
#&gt; SRR1617422     2   0.643     0.5755 0.324 0.588 0.000 0.088
#&gt; SRR1617423     2   0.643     0.5755 0.324 0.588 0.000 0.088
#&gt; SRR1617424     1   0.115     0.6669 0.968 0.024 0.000 0.008
#&gt; SRR1617425     1   0.115     0.6669 0.968 0.024 0.000 0.008
#&gt; SRR1617427     2   0.541     0.6799 0.192 0.728 0.000 0.080
#&gt; SRR1617426     2   0.541     0.6799 0.192 0.728 0.000 0.080
#&gt; SRR1617428     2   0.205     0.7294 0.072 0.924 0.000 0.004
#&gt; SRR1617429     2   0.205     0.7294 0.072 0.924 0.000 0.004
#&gt; SRR1617432     1   0.461     0.6117 0.796 0.040 0.008 0.156
#&gt; SRR1617433     1   0.461     0.6117 0.796 0.040 0.008 0.156
#&gt; SRR1617434     3   0.191     0.7739 0.020 0.000 0.940 0.040
#&gt; SRR1617436     1   0.703     0.2615 0.552 0.296 0.000 0.152
#&gt; SRR1617435     3   0.191     0.7739 0.020 0.000 0.940 0.040
#&gt; SRR1617437     1   0.703     0.2615 0.552 0.296 0.000 0.152
#&gt; SRR1617438     1   0.566     0.5457 0.728 0.004 0.104 0.164
#&gt; SRR1617439     1   0.566     0.5457 0.728 0.004 0.104 0.164
#&gt; SRR1617440     3   0.680     0.6981 0.160 0.004 0.620 0.216
#&gt; SRR1617441     3   0.680     0.6981 0.160 0.004 0.620 0.216
#&gt; SRR1617443     3   0.187     0.7722 0.016 0.012 0.948 0.024
#&gt; SRR1617442     3   0.187     0.7722 0.016 0.012 0.948 0.024
#&gt; SRR1617444     3   0.500     0.7668 0.084 0.000 0.768 0.148
#&gt; SRR1617445     3   0.500     0.7668 0.084 0.000 0.768 0.148
#&gt; SRR1617446     1   0.591     0.4616 0.668 0.252 0.000 0.080
#&gt; SRR1617447     1   0.591     0.4616 0.668 0.252 0.000 0.080
#&gt; SRR1617448     1   0.591     0.4616 0.668 0.252 0.000 0.080
#&gt; SRR1617449     1   0.591     0.4616 0.668 0.252 0.000 0.080
#&gt; SRR1617451     2   0.262     0.7244 0.064 0.908 0.000 0.028
#&gt; SRR1617450     2   0.262     0.7244 0.064 0.908 0.000 0.028
#&gt; SRR1617452     3   0.467     0.7167 0.000 0.036 0.764 0.200
#&gt; SRR1617454     2   0.728     0.2705 0.408 0.444 0.000 0.148
#&gt; SRR1617453     3   0.467     0.7167 0.000 0.036 0.764 0.200
#&gt; SRR1617456     3   0.498     0.7590 0.012 0.000 0.664 0.324
#&gt; SRR1617457     3   0.498     0.7590 0.012 0.000 0.664 0.324
#&gt; SRR1617455     2   0.728     0.2705 0.408 0.444 0.000 0.148
#&gt; SRR1617458     3   0.498     0.7590 0.012 0.000 0.664 0.324
#&gt; SRR1617459     3   0.498     0.7590 0.012 0.000 0.664 0.324
#&gt; SRR1617460     3   0.634     0.7336 0.116 0.004 0.660 0.220
#&gt; SRR1617461     3   0.634     0.7336 0.116 0.004 0.660 0.220
#&gt; SRR1617463     1   0.721    -0.0362 0.516 0.324 0.000 0.160
#&gt; SRR1617462     1   0.721    -0.0362 0.516 0.324 0.000 0.160
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1 p2    p3    p4    p5
#&gt; SRR1617430     4   0.243     0.4296 0.020 NA 0.000 0.912 0.028
#&gt; SRR1617431     4   0.243     0.4296 0.020 NA 0.000 0.912 0.028
#&gt; SRR1617410     1   0.620     0.4702 0.664 NA 0.128 0.000 0.076
#&gt; SRR1617411     1   0.620     0.4702 0.664 NA 0.128 0.000 0.076
#&gt; SRR1617412     1   0.433     0.5473 0.792 NA 0.000 0.016 0.116
#&gt; SRR1617413     1   0.433     0.5473 0.792 NA 0.000 0.016 0.116
#&gt; SRR1617414     5   0.657     1.0000 0.204 NA 0.000 0.388 0.408
#&gt; SRR1617415     5   0.657     1.0000 0.204 NA 0.000 0.388 0.408
#&gt; SRR1617416     3   0.467     0.6066 0.000 NA 0.752 0.008 0.156
#&gt; SRR1617417     3   0.467     0.6066 0.000 NA 0.752 0.008 0.156
#&gt; SRR1617418     3   0.828     0.2253 0.264 NA 0.352 0.004 0.104
#&gt; SRR1617419     3   0.828     0.2253 0.264 NA 0.352 0.004 0.104
#&gt; SRR1617420     1   0.290     0.5683 0.884 NA 0.000 0.012 0.064
#&gt; SRR1617421     1   0.290     0.5683 0.884 NA 0.000 0.012 0.064
#&gt; SRR1617422     4   0.662    -0.5778 0.256 NA 0.000 0.492 0.248
#&gt; SRR1617423     4   0.662    -0.5778 0.256 NA 0.000 0.492 0.248
#&gt; SRR1617424     1   0.152     0.5677 0.952 NA 0.000 0.012 0.020
#&gt; SRR1617425     1   0.152     0.5677 0.952 NA 0.000 0.012 0.020
#&gt; SRR1617427     4   0.645    -0.1541 0.152 NA 0.000 0.596 0.220
#&gt; SRR1617426     4   0.645    -0.1541 0.152 NA 0.000 0.596 0.220
#&gt; SRR1617428     4   0.288     0.3950 0.024 NA 0.000 0.880 0.084
#&gt; SRR1617429     4   0.288     0.3950 0.024 NA 0.000 0.880 0.084
#&gt; SRR1617432     1   0.547     0.2967 0.596 NA 0.016 0.008 0.352
#&gt; SRR1617433     1   0.547     0.2967 0.596 NA 0.016 0.008 0.352
#&gt; SRR1617434     3   0.241     0.6839 0.012 NA 0.908 0.000 0.020
#&gt; SRR1617436     1   0.763     0.0225 0.456 NA 0.000 0.200 0.268
#&gt; SRR1617435     3   0.241     0.6839 0.012 NA 0.908 0.000 0.020
#&gt; SRR1617437     1   0.763     0.0225 0.456 NA 0.000 0.200 0.268
#&gt; SRR1617438     1   0.589     0.4937 0.684 NA 0.080 0.000 0.072
#&gt; SRR1617439     1   0.589     0.4937 0.684 NA 0.080 0.000 0.072
#&gt; SRR1617440     3   0.668     0.6108 0.112 NA 0.484 0.000 0.032
#&gt; SRR1617441     3   0.668     0.6108 0.112 NA 0.484 0.000 0.032
#&gt; SRR1617443     3   0.213     0.6782 0.016 NA 0.928 0.004 0.016
#&gt; SRR1617442     3   0.213     0.6782 0.016 NA 0.928 0.004 0.016
#&gt; SRR1617444     3   0.534     0.6777 0.060 NA 0.648 0.000 0.012
#&gt; SRR1617445     3   0.534     0.6777 0.060 NA 0.648 0.000 0.012
#&gt; SRR1617446     1   0.632     0.3665 0.628 NA 0.000 0.160 0.172
#&gt; SRR1617447     1   0.632     0.3665 0.628 NA 0.000 0.160 0.172
#&gt; SRR1617448     1   0.632     0.3665 0.628 NA 0.000 0.160 0.172
#&gt; SRR1617449     1   0.632     0.3665 0.628 NA 0.000 0.160 0.172
#&gt; SRR1617451     4   0.225     0.4365 0.016 NA 0.000 0.920 0.028
#&gt; SRR1617450     4   0.225     0.4365 0.016 NA 0.000 0.920 0.028
#&gt; SRR1617452     3   0.526     0.5989 0.000 NA 0.708 0.012 0.156
#&gt; SRR1617454     4   0.834    -0.0588 0.296 NA 0.004 0.372 0.156
#&gt; SRR1617453     3   0.526     0.5989 0.000 NA 0.708 0.012 0.156
#&gt; SRR1617456     3   0.456     0.6675 0.000 NA 0.500 0.000 0.008
#&gt; SRR1617457     3   0.456     0.6675 0.000 NA 0.500 0.000 0.008
#&gt; SRR1617455     4   0.834    -0.0588 0.296 NA 0.004 0.372 0.156
#&gt; SRR1617458     3   0.456     0.6675 0.000 NA 0.500 0.000 0.008
#&gt; SRR1617459     3   0.456     0.6675 0.000 NA 0.500 0.000 0.008
#&gt; SRR1617460     3   0.620     0.6325 0.092 NA 0.504 0.000 0.016
#&gt; SRR1617461     3   0.620     0.6325 0.092 NA 0.504 0.000 0.016
#&gt; SRR1617463     1   0.829    -0.1099 0.416 NA 0.004 0.232 0.176
#&gt; SRR1617462     1   0.829    -0.1099 0.416 NA 0.004 0.232 0.176
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2   0.243     0.4113 0.012 0.900 0.036 0.048 0.004 0.000
#&gt; SRR1617431     2   0.243     0.4113 0.012 0.900 0.036 0.048 0.004 0.000
#&gt; SRR1617410     1   0.644    -0.0200 0.556 0.000 0.160 0.012 0.048 0.224
#&gt; SRR1617411     1   0.644    -0.0200 0.556 0.000 0.160 0.012 0.048 0.224
#&gt; SRR1617412     1   0.513     0.4365 0.716 0.008 0.140 0.036 0.096 0.004
#&gt; SRR1617413     1   0.513     0.4365 0.716 0.008 0.140 0.036 0.096 0.004
#&gt; SRR1617414     5   0.585     1.0000 0.172 0.356 0.004 0.000 0.468 0.000
#&gt; SRR1617415     5   0.585     1.0000 0.172 0.356 0.004 0.000 0.468 0.000
#&gt; SRR1617416     4   0.417     0.8556 0.000 0.000 0.008 0.692 0.028 0.272
#&gt; SRR1617417     4   0.417     0.8556 0.000 0.000 0.008 0.692 0.028 0.272
#&gt; SRR1617418     3   0.667     1.0000 0.172 0.000 0.508 0.084 0.000 0.236
#&gt; SRR1617419     3   0.667     1.0000 0.172 0.000 0.508 0.084 0.000 0.236
#&gt; SRR1617420     1   0.288     0.4644 0.880 0.008 0.064 0.004 0.028 0.016
#&gt; SRR1617421     1   0.288     0.4644 0.880 0.008 0.064 0.004 0.028 0.016
#&gt; SRR1617422     2   0.642    -0.4695 0.232 0.476 0.016 0.008 0.268 0.000
#&gt; SRR1617423     2   0.642    -0.4695 0.232 0.476 0.016 0.008 0.268 0.000
#&gt; SRR1617424     1   0.258     0.4873 0.896 0.008 0.020 0.000 0.044 0.032
#&gt; SRR1617425     1   0.258     0.4873 0.896 0.008 0.020 0.000 0.044 0.032
#&gt; SRR1617427     2   0.611     0.0301 0.120 0.592 0.044 0.012 0.232 0.000
#&gt; SRR1617426     2   0.611     0.0301 0.120 0.592 0.044 0.012 0.232 0.000
#&gt; SRR1617428     2   0.281     0.3749 0.016 0.872 0.016 0.008 0.088 0.000
#&gt; SRR1617429     2   0.281     0.3749 0.016 0.872 0.016 0.008 0.088 0.000
#&gt; SRR1617432     1   0.550     0.1387 0.528 0.000 0.016 0.008 0.384 0.064
#&gt; SRR1617433     1   0.550     0.1387 0.528 0.000 0.016 0.008 0.384 0.064
#&gt; SRR1617434     6   0.561     0.0712 0.016 0.000 0.052 0.332 0.028 0.572
#&gt; SRR1617436     1   0.791     0.0775 0.392 0.192 0.168 0.028 0.220 0.000
#&gt; SRR1617435     6   0.561     0.0712 0.016 0.000 0.052 0.332 0.028 0.572
#&gt; SRR1617437     1   0.791     0.0775 0.392 0.192 0.168 0.028 0.220 0.000
#&gt; SRR1617438     1   0.643     0.0540 0.556 0.000 0.180 0.012 0.044 0.208
#&gt; SRR1617439     1   0.643     0.0540 0.556 0.000 0.180 0.012 0.044 0.208
#&gt; SRR1617440     6   0.409     0.2903 0.100 0.000 0.112 0.008 0.004 0.776
#&gt; SRR1617441     6   0.409     0.2903 0.100 0.000 0.112 0.008 0.004 0.776
#&gt; SRR1617443     6   0.591    -0.0976 0.016 0.000 0.080 0.376 0.020 0.508
#&gt; SRR1617442     6   0.591    -0.0976 0.016 0.000 0.080 0.376 0.020 0.508
#&gt; SRR1617444     6   0.332     0.4774 0.044 0.000 0.028 0.076 0.004 0.848
#&gt; SRR1617445     6   0.332     0.4774 0.044 0.000 0.028 0.076 0.004 0.848
#&gt; SRR1617446     1   0.679     0.3072 0.544 0.148 0.060 0.028 0.220 0.000
#&gt; SRR1617447     1   0.679     0.3072 0.544 0.148 0.060 0.028 0.220 0.000
#&gt; SRR1617448     1   0.679     0.3072 0.544 0.148 0.060 0.028 0.220 0.000
#&gt; SRR1617449     1   0.679     0.3072 0.544 0.148 0.060 0.028 0.220 0.000
#&gt; SRR1617451     2   0.258     0.4153 0.012 0.896 0.020 0.020 0.052 0.000
#&gt; SRR1617450     2   0.258     0.4153 0.012 0.896 0.020 0.020 0.052 0.000
#&gt; SRR1617452     4   0.340     0.8596 0.000 0.004 0.000 0.776 0.016 0.204
#&gt; SRR1617454     2   0.882    -0.0451 0.264 0.308 0.048 0.044 0.220 0.116
#&gt; SRR1617453     4   0.340     0.8596 0.000 0.004 0.000 0.776 0.016 0.204
#&gt; SRR1617456     6   0.617     0.3744 0.004 0.004 0.096 0.192 0.092 0.612
#&gt; SRR1617457     6   0.617     0.3744 0.004 0.004 0.096 0.192 0.092 0.612
#&gt; SRR1617455     2   0.882    -0.0451 0.264 0.308 0.048 0.044 0.220 0.116
#&gt; SRR1617458     6   0.626     0.3737 0.004 0.004 0.096 0.192 0.100 0.604
#&gt; SRR1617459     6   0.626     0.3737 0.004 0.004 0.096 0.192 0.100 0.604
#&gt; SRR1617460     6   0.313     0.4977 0.036 0.004 0.028 0.012 0.048 0.872
#&gt; SRR1617461     6   0.313     0.4977 0.036 0.004 0.028 0.012 0.048 0.872
#&gt; SRR1617463     1   0.875    -0.1167 0.352 0.200 0.060 0.036 0.236 0.116
#&gt; SRR1617462     1   0.875    -0.1167 0.352 0.200 0.060 0.036 0.236 0.116
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.5036 0.497   0.497
#> 3 3 0.759           0.870       0.934         0.3223 0.760   0.547
#> 4 4 0.753           0.677       0.791         0.0944 0.927   0.781
#> 5 5 0.677           0.638       0.761         0.0596 0.925   0.740
#> 6 6 0.719           0.643       0.744         0.0390 0.939   0.758
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1 p2
#&gt; SRR1617430     1       0          1  1  0
#&gt; SRR1617431     1       0          1  1  0
#&gt; SRR1617410     2       0          1  0  1
#&gt; SRR1617411     2       0          1  0  1
#&gt; SRR1617412     1       0          1  1  0
#&gt; SRR1617413     1       0          1  1  0
#&gt; SRR1617414     1       0          1  1  0
#&gt; SRR1617415     1       0          1  1  0
#&gt; SRR1617416     2       0          1  0  1
#&gt; SRR1617417     2       0          1  0  1
#&gt; SRR1617418     2       0          1  0  1
#&gt; SRR1617419     2       0          1  0  1
#&gt; SRR1617420     1       0          1  1  0
#&gt; SRR1617421     1       0          1  1  0
#&gt; SRR1617422     1       0          1  1  0
#&gt; SRR1617423     1       0          1  1  0
#&gt; SRR1617424     1       0          1  1  0
#&gt; SRR1617425     1       0          1  1  0
#&gt; SRR1617427     1       0          1  1  0
#&gt; SRR1617426     1       0          1  1  0
#&gt; SRR1617428     1       0          1  1  0
#&gt; SRR1617429     1       0          1  1  0
#&gt; SRR1617432     1       0          1  1  0
#&gt; SRR1617433     1       0          1  1  0
#&gt; SRR1617434     2       0          1  0  1
#&gt; SRR1617436     1       0          1  1  0
#&gt; SRR1617435     2       0          1  0  1
#&gt; SRR1617437     1       0          1  1  0
#&gt; SRR1617438     2       0          1  0  1
#&gt; SRR1617439     2       0          1  0  1
#&gt; SRR1617440     2       0          1  0  1
#&gt; SRR1617441     2       0          1  0  1
#&gt; SRR1617443     2       0          1  0  1
#&gt; SRR1617442     2       0          1  0  1
#&gt; SRR1617444     2       0          1  0  1
#&gt; SRR1617445     2       0          1  0  1
#&gt; SRR1617446     1       0          1  1  0
#&gt; SRR1617447     1       0          1  1  0
#&gt; SRR1617448     1       0          1  1  0
#&gt; SRR1617449     1       0          1  1  0
#&gt; SRR1617451     1       0          1  1  0
#&gt; SRR1617450     1       0          1  1  0
#&gt; SRR1617452     2       0          1  0  1
#&gt; SRR1617454     1       0          1  1  0
#&gt; SRR1617453     2       0          1  0  1
#&gt; SRR1617456     2       0          1  0  1
#&gt; SRR1617457     2       0          1  0  1
#&gt; SRR1617455     1       0          1  1  0
#&gt; SRR1617458     2       0          1  0  1
#&gt; SRR1617459     2       0          1  0  1
#&gt; SRR1617460     2       0          1  0  1
#&gt; SRR1617461     2       0          1  0  1
#&gt; SRR1617463     1       0          1  1  0
#&gt; SRR1617462     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617431     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617410     1  0.6225      0.270 0.568 0.000 0.432
#&gt; SRR1617411     1  0.6215      0.280 0.572 0.000 0.428
#&gt; SRR1617412     1  0.0237      0.808 0.996 0.004 0.000
#&gt; SRR1617413     1  0.0237      0.808 0.996 0.004 0.000
#&gt; SRR1617414     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617415     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617416     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617417     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617418     3  0.4291      0.756 0.180 0.000 0.820
#&gt; SRR1617419     3  0.4291      0.756 0.180 0.000 0.820
#&gt; SRR1617420     1  0.0000      0.806 1.000 0.000 0.000
#&gt; SRR1617421     1  0.0000      0.806 1.000 0.000 0.000
#&gt; SRR1617422     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617423     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617424     1  0.0592      0.808 0.988 0.012 0.000
#&gt; SRR1617425     1  0.0592      0.808 0.988 0.012 0.000
#&gt; SRR1617427     2  0.0424      0.962 0.008 0.992 0.000
#&gt; SRR1617426     2  0.0424      0.962 0.008 0.992 0.000
#&gt; SRR1617428     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617429     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617432     2  0.5058      0.691 0.244 0.756 0.000
#&gt; SRR1617433     2  0.5058      0.691 0.244 0.756 0.000
#&gt; SRR1617434     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617436     1  0.5835      0.582 0.660 0.340 0.000
#&gt; SRR1617435     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617437     1  0.5835      0.582 0.660 0.340 0.000
#&gt; SRR1617438     1  0.4452      0.699 0.808 0.000 0.192
#&gt; SRR1617439     1  0.4452      0.699 0.808 0.000 0.192
#&gt; SRR1617440     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617441     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617443     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617442     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617444     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617445     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617446     1  0.4291      0.761 0.820 0.180 0.000
#&gt; SRR1617447     1  0.4291      0.761 0.820 0.180 0.000
#&gt; SRR1617448     1  0.4291      0.761 0.820 0.180 0.000
#&gt; SRR1617449     1  0.4291      0.761 0.820 0.180 0.000
#&gt; SRR1617451     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617450     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617452     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617454     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617453     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617456     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617457     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617455     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617458     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617459     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617460     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617461     3  0.0000      0.978 0.000 0.000 1.000
#&gt; SRR1617463     2  0.0000      0.968 0.000 1.000 0.000
#&gt; SRR1617462     2  0.0000      0.968 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.0469     0.9211 0.012 0.988 0.000 0.000
#&gt; SRR1617431     2  0.0469     0.9211 0.012 0.988 0.000 0.000
#&gt; SRR1617410     1  0.7090     0.0626 0.496 0.000 0.372 0.132
#&gt; SRR1617411     1  0.7090     0.0626 0.496 0.000 0.372 0.132
#&gt; SRR1617412     3  0.4485     0.4713 0.248 0.012 0.740 0.000
#&gt; SRR1617413     3  0.4485     0.4713 0.248 0.012 0.740 0.000
#&gt; SRR1617414     2  0.3528     0.7951 0.192 0.808 0.000 0.000
#&gt; SRR1617415     2  0.3528     0.7951 0.192 0.808 0.000 0.000
#&gt; SRR1617416     4  0.0779     0.9325 0.016 0.000 0.004 0.980
#&gt; SRR1617417     4  0.0779     0.9325 0.016 0.000 0.004 0.980
#&gt; SRR1617418     4  0.5744     0.3408 0.028 0.000 0.436 0.536
#&gt; SRR1617419     4  0.5744     0.3408 0.028 0.000 0.436 0.536
#&gt; SRR1617420     3  0.5000     0.3100 0.496 0.000 0.504 0.000
#&gt; SRR1617421     3  0.5000     0.3100 0.496 0.000 0.504 0.000
#&gt; SRR1617422     2  0.0592     0.9210 0.016 0.984 0.000 0.000
#&gt; SRR1617423     2  0.0592     0.9210 0.016 0.984 0.000 0.000
#&gt; SRR1617424     3  0.5700     0.2372 0.412 0.028 0.560 0.000
#&gt; SRR1617425     3  0.5700     0.2372 0.412 0.028 0.560 0.000
#&gt; SRR1617427     2  0.4798     0.7282 0.180 0.768 0.052 0.000
#&gt; SRR1617426     2  0.4798     0.7282 0.180 0.768 0.052 0.000
#&gt; SRR1617428     2  0.0592     0.9210 0.016 0.984 0.000 0.000
#&gt; SRR1617429     2  0.0592     0.9210 0.016 0.984 0.000 0.000
#&gt; SRR1617432     1  0.3711     0.3038 0.836 0.140 0.024 0.000
#&gt; SRR1617433     1  0.3711     0.3038 0.836 0.140 0.024 0.000
#&gt; SRR1617434     4  0.0895     0.9311 0.020 0.000 0.004 0.976
#&gt; SRR1617436     3  0.7574     0.1667 0.248 0.268 0.484 0.000
#&gt; SRR1617435     4  0.0895     0.9311 0.020 0.000 0.004 0.976
#&gt; SRR1617437     3  0.7574     0.1667 0.248 0.268 0.484 0.000
#&gt; SRR1617438     3  0.1452     0.4135 0.008 0.000 0.956 0.036
#&gt; SRR1617439     3  0.1452     0.4135 0.008 0.000 0.956 0.036
#&gt; SRR1617440     4  0.1174     0.9284 0.012 0.000 0.020 0.968
#&gt; SRR1617441     4  0.1174     0.9284 0.012 0.000 0.020 0.968
#&gt; SRR1617443     4  0.0779     0.9325 0.016 0.000 0.004 0.980
#&gt; SRR1617442     4  0.0779     0.9325 0.016 0.000 0.004 0.980
#&gt; SRR1617444     4  0.0000     0.9362 0.000 0.000 0.000 1.000
#&gt; SRR1617445     4  0.0000     0.9362 0.000 0.000 0.000 1.000
#&gt; SRR1617446     1  0.6444     0.2757 0.612 0.104 0.284 0.000
#&gt; SRR1617447     1  0.6444     0.2757 0.612 0.104 0.284 0.000
#&gt; SRR1617448     1  0.6444     0.2757 0.612 0.104 0.284 0.000
#&gt; SRR1617449     1  0.6444     0.2757 0.612 0.104 0.284 0.000
#&gt; SRR1617451     2  0.0000     0.9191 0.000 1.000 0.000 0.000
#&gt; SRR1617450     2  0.0000     0.9191 0.000 1.000 0.000 0.000
#&gt; SRR1617452     4  0.0000     0.9362 0.000 0.000 0.000 1.000
#&gt; SRR1617454     2  0.1406     0.9036 0.024 0.960 0.016 0.000
#&gt; SRR1617453     4  0.0000     0.9362 0.000 0.000 0.000 1.000
#&gt; SRR1617456     4  0.0592     0.9343 0.016 0.000 0.000 0.984
#&gt; SRR1617457     4  0.0592     0.9343 0.016 0.000 0.000 0.984
#&gt; SRR1617455     2  0.1406     0.9036 0.024 0.960 0.016 0.000
#&gt; SRR1617458     4  0.0592     0.9343 0.016 0.000 0.000 0.984
#&gt; SRR1617459     4  0.0592     0.9343 0.016 0.000 0.000 0.984
#&gt; SRR1617460     4  0.0592     0.9343 0.016 0.000 0.000 0.984
#&gt; SRR1617461     4  0.0592     0.9343 0.016 0.000 0.000 0.984
#&gt; SRR1617463     2  0.1406     0.9036 0.024 0.960 0.016 0.000
#&gt; SRR1617462     2  0.1406     0.9036 0.024 0.960 0.016 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     4  0.0162      0.796 0.004 0.000 0.000 0.996 0.000
#&gt; SRR1617431     4  0.0162      0.796 0.004 0.000 0.000 0.996 0.000
#&gt; SRR1617410     5  0.8022      0.300 0.192 0.112 0.304 0.000 0.392
#&gt; SRR1617411     5  0.8022      0.300 0.192 0.112 0.304 0.000 0.392
#&gt; SRR1617412     1  0.4909      0.354 0.560 0.000 0.412 0.000 0.028
#&gt; SRR1617413     1  0.4909      0.354 0.560 0.000 0.412 0.000 0.028
#&gt; SRR1617414     4  0.5533      0.354 0.060 0.000 0.004 0.540 0.396
#&gt; SRR1617415     4  0.5533      0.354 0.060 0.000 0.004 0.540 0.396
#&gt; SRR1617416     2  0.1211      0.890 0.000 0.960 0.024 0.000 0.016
#&gt; SRR1617417     2  0.1211      0.890 0.000 0.960 0.024 0.000 0.016
#&gt; SRR1617418     3  0.6562      0.165 0.028 0.368 0.496 0.000 0.108
#&gt; SRR1617419     3  0.6562      0.165 0.028 0.368 0.496 0.000 0.108
#&gt; SRR1617420     3  0.6685      0.128 0.280 0.000 0.436 0.000 0.284
#&gt; SRR1617421     3  0.6685      0.128 0.280 0.000 0.436 0.000 0.284
#&gt; SRR1617422     4  0.2238      0.777 0.020 0.000 0.004 0.912 0.064
#&gt; SRR1617423     4  0.2238      0.777 0.020 0.000 0.004 0.912 0.064
#&gt; SRR1617424     1  0.4969      0.501 0.752 0.000 0.116 0.028 0.104
#&gt; SRR1617425     1  0.4969      0.501 0.752 0.000 0.116 0.028 0.104
#&gt; SRR1617427     4  0.5576      0.463 0.268 0.000 0.004 0.628 0.100
#&gt; SRR1617426     4  0.5576      0.463 0.268 0.000 0.004 0.628 0.100
#&gt; SRR1617428     4  0.1018      0.794 0.016 0.000 0.000 0.968 0.016
#&gt; SRR1617429     4  0.1018      0.794 0.016 0.000 0.000 0.968 0.016
#&gt; SRR1617432     5  0.5563      0.465 0.244 0.004 0.008 0.088 0.656
#&gt; SRR1617433     5  0.5563      0.465 0.244 0.004 0.008 0.088 0.656
#&gt; SRR1617434     2  0.1403      0.887 0.000 0.952 0.024 0.000 0.024
#&gt; SRR1617436     1  0.7370      0.403 0.404 0.000 0.316 0.248 0.032
#&gt; SRR1617435     2  0.1403      0.887 0.000 0.952 0.024 0.000 0.024
#&gt; SRR1617437     1  0.7370      0.403 0.404 0.000 0.316 0.248 0.032
#&gt; SRR1617438     3  0.2464      0.372 0.096 0.016 0.888 0.000 0.000
#&gt; SRR1617439     3  0.2464      0.372 0.096 0.016 0.888 0.000 0.000
#&gt; SRR1617440     2  0.3772      0.796 0.000 0.792 0.172 0.000 0.036
#&gt; SRR1617441     2  0.3772      0.796 0.000 0.792 0.172 0.000 0.036
#&gt; SRR1617443     2  0.1469      0.887 0.000 0.948 0.036 0.000 0.016
#&gt; SRR1617442     2  0.1469      0.887 0.000 0.948 0.036 0.000 0.016
#&gt; SRR1617444     2  0.0000      0.900 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617445     2  0.0000      0.900 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617446     1  0.2573      0.625 0.880 0.000 0.000 0.104 0.016
#&gt; SRR1617447     1  0.2573      0.625 0.880 0.000 0.000 0.104 0.016
#&gt; SRR1617448     1  0.2573      0.625 0.880 0.000 0.000 0.104 0.016
#&gt; SRR1617449     1  0.2573      0.625 0.880 0.000 0.000 0.104 0.016
#&gt; SRR1617451     4  0.0880      0.791 0.000 0.000 0.000 0.968 0.032
#&gt; SRR1617450     4  0.0880      0.791 0.000 0.000 0.000 0.968 0.032
#&gt; SRR1617452     2  0.0000      0.900 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617454     4  0.3942      0.718 0.016 0.000 0.032 0.804 0.148
#&gt; SRR1617453     2  0.0000      0.900 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1617456     2  0.3180      0.869 0.000 0.856 0.076 0.000 0.068
#&gt; SRR1617457     2  0.3180      0.869 0.000 0.856 0.076 0.000 0.068
#&gt; SRR1617455     4  0.3942      0.718 0.016 0.000 0.032 0.804 0.148
#&gt; SRR1617458     2  0.3180      0.869 0.000 0.856 0.076 0.000 0.068
#&gt; SRR1617459     2  0.3180      0.869 0.000 0.856 0.076 0.000 0.068
#&gt; SRR1617460     2  0.3242      0.865 0.000 0.852 0.076 0.000 0.072
#&gt; SRR1617461     2  0.3242      0.865 0.000 0.852 0.076 0.000 0.072
#&gt; SRR1617463     4  0.4062      0.715 0.016 0.000 0.036 0.796 0.152
#&gt; SRR1617462     4  0.4062      0.715 0.016 0.000 0.036 0.796 0.152
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.0547      0.737 0.020 0.980 0.000 0.000 0.000 0.000
#&gt; SRR1617431     2  0.0547      0.737 0.020 0.980 0.000 0.000 0.000 0.000
#&gt; SRR1617410     4  0.7903      0.649 0.084 0.000 0.112 0.456 0.200 0.148
#&gt; SRR1617411     4  0.7903      0.649 0.084 0.000 0.112 0.456 0.200 0.148
#&gt; SRR1617412     3  0.4697      0.433 0.396 0.004 0.568 0.020 0.012 0.000
#&gt; SRR1617413     3  0.4697      0.433 0.396 0.004 0.568 0.020 0.012 0.000
#&gt; SRR1617414     5  0.4076      0.573 0.012 0.396 0.000 0.000 0.592 0.000
#&gt; SRR1617415     5  0.4076      0.573 0.012 0.396 0.000 0.000 0.592 0.000
#&gt; SRR1617416     6  0.1141      0.788 0.000 0.000 0.000 0.052 0.000 0.948
#&gt; SRR1617417     6  0.1141      0.788 0.000 0.000 0.000 0.052 0.000 0.948
#&gt; SRR1617418     4  0.6399      0.663 0.012 0.000 0.272 0.436 0.004 0.276
#&gt; SRR1617419     4  0.6399      0.663 0.012 0.000 0.272 0.436 0.004 0.276
#&gt; SRR1617420     3  0.6066      0.509 0.196 0.000 0.580 0.048 0.176 0.000
#&gt; SRR1617421     3  0.6066      0.509 0.196 0.000 0.580 0.048 0.176 0.000
#&gt; SRR1617422     2  0.2208      0.703 0.032 0.908 0.004 0.004 0.052 0.000
#&gt; SRR1617423     2  0.2208      0.703 0.032 0.908 0.004 0.004 0.052 0.000
#&gt; SRR1617424     1  0.5760      0.513 0.632 0.012 0.108 0.212 0.036 0.000
#&gt; SRR1617425     1  0.5760      0.513 0.632 0.012 0.108 0.212 0.036 0.000
#&gt; SRR1617427     2  0.4995      0.363 0.252 0.652 0.008 0.004 0.084 0.000
#&gt; SRR1617426     2  0.4995      0.363 0.252 0.652 0.008 0.004 0.084 0.000
#&gt; SRR1617428     2  0.1049      0.733 0.032 0.960 0.000 0.000 0.008 0.000
#&gt; SRR1617429     2  0.1049      0.733 0.032 0.960 0.000 0.000 0.008 0.000
#&gt; SRR1617432     5  0.2933      0.605 0.096 0.040 0.008 0.000 0.856 0.000
#&gt; SRR1617433     5  0.2933      0.605 0.096 0.040 0.008 0.000 0.856 0.000
#&gt; SRR1617434     6  0.1349      0.783 0.000 0.000 0.000 0.056 0.004 0.940
#&gt; SRR1617436     3  0.6062      0.310 0.320 0.232 0.444 0.000 0.004 0.000
#&gt; SRR1617435     6  0.1349      0.783 0.000 0.000 0.000 0.056 0.004 0.940
#&gt; SRR1617437     3  0.6062      0.310 0.320 0.232 0.444 0.000 0.004 0.000
#&gt; SRR1617438     3  0.2723      0.362 0.008 0.000 0.872 0.096 0.008 0.016
#&gt; SRR1617439     3  0.2723      0.362 0.008 0.000 0.872 0.096 0.008 0.016
#&gt; SRR1617440     6  0.5061      0.606 0.000 0.000 0.132 0.204 0.008 0.656
#&gt; SRR1617441     6  0.5061      0.606 0.000 0.000 0.132 0.204 0.008 0.656
#&gt; SRR1617443     6  0.1049      0.796 0.000 0.000 0.008 0.032 0.000 0.960
#&gt; SRR1617442     6  0.1049      0.796 0.000 0.000 0.008 0.032 0.000 0.960
#&gt; SRR1617444     6  0.0000      0.808 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617445     6  0.0000      0.808 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1617446     1  0.2068      0.774 0.904 0.080 0.008 0.000 0.008 0.000
#&gt; SRR1617447     1  0.2068      0.774 0.904 0.080 0.008 0.000 0.008 0.000
#&gt; SRR1617448     1  0.2068      0.774 0.904 0.080 0.008 0.000 0.008 0.000
#&gt; SRR1617449     1  0.2068      0.774 0.904 0.080 0.008 0.000 0.008 0.000
#&gt; SRR1617451     2  0.1138      0.731 0.000 0.960 0.004 0.012 0.024 0.000
#&gt; SRR1617450     2  0.1138      0.731 0.000 0.960 0.004 0.012 0.024 0.000
#&gt; SRR1617452     6  0.0146      0.809 0.000 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1617454     2  0.5633      0.568 0.028 0.652 0.016 0.188 0.116 0.000
#&gt; SRR1617453     6  0.0146      0.809 0.000 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1617456     6  0.3476      0.748 0.000 0.000 0.004 0.260 0.004 0.732
#&gt; SRR1617457     6  0.3476      0.748 0.000 0.000 0.004 0.260 0.004 0.732
#&gt; SRR1617455     2  0.5633      0.568 0.028 0.652 0.016 0.188 0.116 0.000
#&gt; SRR1617458     6  0.3476      0.748 0.000 0.000 0.004 0.260 0.004 0.732
#&gt; SRR1617459     6  0.3476      0.748 0.000 0.000 0.004 0.260 0.004 0.732
#&gt; SRR1617460     6  0.3746      0.731 0.000 0.000 0.004 0.272 0.012 0.712
#&gt; SRR1617461     6  0.3746      0.731 0.000 0.000 0.004 0.272 0.012 0.712
#&gt; SRR1617463     2  0.5776      0.556 0.028 0.632 0.016 0.204 0.120 0.000
#&gt; SRR1617462     2  0.5776      0.556 0.028 0.632 0.016 0.204 0.120 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.673           0.879       0.944         0.4820 0.497   0.497
#> 3 3 0.944           0.940       0.973         0.2965 0.709   0.507
#> 4 4 0.890           0.896       0.948         0.0673 0.961   0.900
#> 5 5 0.662           0.794       0.840         0.0723 0.972   0.920
#> 6 6 0.733           0.667       0.851         0.0997 0.888   0.654
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     1   0.000      0.990 1.000 0.000
#&gt; SRR1617431     1   0.000      0.990 1.000 0.000
#&gt; SRR1617410     2   0.978      0.443 0.412 0.588
#&gt; SRR1617411     2   0.978      0.443 0.412 0.588
#&gt; SRR1617412     1   0.000      0.990 1.000 0.000
#&gt; SRR1617413     1   0.000      0.990 1.000 0.000
#&gt; SRR1617414     1   0.000      0.990 1.000 0.000
#&gt; SRR1617415     1   0.000      0.990 1.000 0.000
#&gt; SRR1617416     2   0.000      0.869 0.000 1.000
#&gt; SRR1617417     2   0.000      0.869 0.000 1.000
#&gt; SRR1617418     2   0.978      0.443 0.412 0.588
#&gt; SRR1617419     2   0.978      0.443 0.412 0.588
#&gt; SRR1617420     1   0.000      0.990 1.000 0.000
#&gt; SRR1617421     1   0.000      0.990 1.000 0.000
#&gt; SRR1617422     1   0.000      0.990 1.000 0.000
#&gt; SRR1617423     1   0.000      0.990 1.000 0.000
#&gt; SRR1617424     1   0.000      0.990 1.000 0.000
#&gt; SRR1617425     1   0.000      0.990 1.000 0.000
#&gt; SRR1617427     1   0.000      0.990 1.000 0.000
#&gt; SRR1617426     1   0.000      0.990 1.000 0.000
#&gt; SRR1617428     1   0.000      0.990 1.000 0.000
#&gt; SRR1617429     1   0.000      0.990 1.000 0.000
#&gt; SRR1617432     1   0.574      0.813 0.864 0.136
#&gt; SRR1617433     1   0.469      0.867 0.900 0.100
#&gt; SRR1617434     2   0.000      0.869 0.000 1.000
#&gt; SRR1617436     1   0.000      0.990 1.000 0.000
#&gt; SRR1617435     2   0.000      0.869 0.000 1.000
#&gt; SRR1617437     1   0.000      0.990 1.000 0.000
#&gt; SRR1617438     2   0.978      0.443 0.412 0.588
#&gt; SRR1617439     2   0.978      0.443 0.412 0.588
#&gt; SRR1617440     2   0.000      0.869 0.000 1.000
#&gt; SRR1617441     2   0.000      0.869 0.000 1.000
#&gt; SRR1617443     2   0.000      0.869 0.000 1.000
#&gt; SRR1617442     2   0.000      0.869 0.000 1.000
#&gt; SRR1617444     2   0.000      0.869 0.000 1.000
#&gt; SRR1617445     2   0.000      0.869 0.000 1.000
#&gt; SRR1617446     1   0.000      0.990 1.000 0.000
#&gt; SRR1617447     1   0.000      0.990 1.000 0.000
#&gt; SRR1617448     1   0.000      0.990 1.000 0.000
#&gt; SRR1617449     1   0.000      0.990 1.000 0.000
#&gt; SRR1617451     1   0.000      0.990 1.000 0.000
#&gt; SRR1617450     1   0.000      0.990 1.000 0.000
#&gt; SRR1617452     2   0.000      0.869 0.000 1.000
#&gt; SRR1617454     1   0.000      0.990 1.000 0.000
#&gt; SRR1617453     2   0.000      0.869 0.000 1.000
#&gt; SRR1617456     2   0.000      0.869 0.000 1.000
#&gt; SRR1617457     2   0.000      0.869 0.000 1.000
#&gt; SRR1617455     1   0.000      0.990 1.000 0.000
#&gt; SRR1617458     2   0.000      0.869 0.000 1.000
#&gt; SRR1617459     2   0.000      0.869 0.000 1.000
#&gt; SRR1617460     2   0.605      0.762 0.148 0.852
#&gt; SRR1617461     2   0.605      0.762 0.148 0.852
#&gt; SRR1617463     1   0.000      0.990 1.000 0.000
#&gt; SRR1617462     1   0.000      0.990 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1617431     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1617410     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617411     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617412     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617413     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617414     1  0.4796      0.735 0.780 0.220 0.000
#&gt; SRR1617415     1  0.4974      0.713 0.764 0.236 0.000
#&gt; SRR1617416     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617417     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617418     1  0.2261      0.888 0.932 0.000 0.068
#&gt; SRR1617419     1  0.3482      0.827 0.872 0.000 0.128
#&gt; SRR1617420     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617421     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617422     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617427     1  0.6126      0.416 0.600 0.400 0.000
#&gt; SRR1617426     1  0.6126      0.416 0.600 0.400 0.000
#&gt; SRR1617428     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1617429     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1617432     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617433     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617434     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617436     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617435     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617437     1  0.0237      0.942 0.996 0.004 0.000
#&gt; SRR1617438     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617439     1  0.0237      0.941 0.996 0.000 0.004
#&gt; SRR1617440     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617441     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617443     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617442     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617444     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617445     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617446     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617447     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617448     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617449     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617451     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1617450     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1617452     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617454     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1617453     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617456     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617457     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617455     2  0.0000      1.000 0.000 1.000 0.000
#&gt; SRR1617458     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617459     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617460     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617461     3  0.0000      1.000 0.000 0.000 1.000
#&gt; SRR1617463     1  0.0000      0.944 1.000 0.000 0.000
#&gt; SRR1617462     1  0.0000      0.944 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; SRR1617431     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; SRR1617410     1  0.0336      0.936 0.992 0.000 0.008 0.000
#&gt; SRR1617411     1  0.0336      0.936 0.992 0.000 0.008 0.000
#&gt; SRR1617412     1  0.0336      0.938 0.992 0.000 0.000 0.008
#&gt; SRR1617413     1  0.0336      0.938 0.992 0.000 0.000 0.008
#&gt; SRR1617414     1  0.3764      0.739 0.784 0.216 0.000 0.000
#&gt; SRR1617415     1  0.3907      0.718 0.768 0.232 0.000 0.000
#&gt; SRR1617416     4  0.3649      0.833 0.000 0.000 0.204 0.796
#&gt; SRR1617417     4  0.3649      0.833 0.000 0.000 0.204 0.796
#&gt; SRR1617418     1  0.1940      0.881 0.924 0.000 0.076 0.000
#&gt; SRR1617419     1  0.2868      0.818 0.864 0.000 0.136 0.000
#&gt; SRR1617420     1  0.0000      0.938 1.000 0.000 0.000 0.000
#&gt; SRR1617421     1  0.0000      0.938 1.000 0.000 0.000 0.000
#&gt; SRR1617422     1  0.0000      0.938 1.000 0.000 0.000 0.000
#&gt; SRR1617423     1  0.0000      0.938 1.000 0.000 0.000 0.000
#&gt; SRR1617424     1  0.0000      0.938 1.000 0.000 0.000 0.000
#&gt; SRR1617425     1  0.0000      0.938 1.000 0.000 0.000 0.000
#&gt; SRR1617427     1  0.5016      0.426 0.600 0.396 0.000 0.004
#&gt; SRR1617426     1  0.5016      0.426 0.600 0.396 0.000 0.004
#&gt; SRR1617428     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; SRR1617429     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; SRR1617432     1  0.0000      0.938 1.000 0.000 0.000 0.000
#&gt; SRR1617433     1  0.0000      0.938 1.000 0.000 0.000 0.000
#&gt; SRR1617434     3  0.0000      0.926 0.000 0.000 1.000 0.000
#&gt; SRR1617436     1  0.0336      0.938 0.992 0.000 0.000 0.008
#&gt; SRR1617435     3  0.0000      0.926 0.000 0.000 1.000 0.000
#&gt; SRR1617437     1  0.0524      0.937 0.988 0.004 0.000 0.008
#&gt; SRR1617438     1  0.0336      0.936 0.992 0.000 0.008 0.000
#&gt; SRR1617439     1  0.0469      0.934 0.988 0.000 0.012 0.000
#&gt; SRR1617440     3  0.0000      0.926 0.000 0.000 1.000 0.000
#&gt; SRR1617441     3  0.0000      0.926 0.000 0.000 1.000 0.000
#&gt; SRR1617443     3  0.0000      0.926 0.000 0.000 1.000 0.000
#&gt; SRR1617442     3  0.0000      0.926 0.000 0.000 1.000 0.000
#&gt; SRR1617444     3  0.0000      0.926 0.000 0.000 1.000 0.000
#&gt; SRR1617445     3  0.0000      0.926 0.000 0.000 1.000 0.000
#&gt; SRR1617446     1  0.0336      0.938 0.992 0.000 0.000 0.008
#&gt; SRR1617447     1  0.0336      0.938 0.992 0.000 0.000 0.008
#&gt; SRR1617448     1  0.0336      0.938 0.992 0.000 0.000 0.008
#&gt; SRR1617449     1  0.0336      0.938 0.992 0.000 0.000 0.008
#&gt; SRR1617451     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; SRR1617450     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; SRR1617452     4  0.0336      0.839 0.000 0.000 0.008 0.992
#&gt; SRR1617454     2  0.0336      0.991 0.008 0.992 0.000 0.000
#&gt; SRR1617453     4  0.0336      0.839 0.000 0.000 0.008 0.992
#&gt; SRR1617456     3  0.3569      0.806 0.000 0.000 0.804 0.196
#&gt; SRR1617457     3  0.3569      0.806 0.000 0.000 0.804 0.196
#&gt; SRR1617455     2  0.0336      0.991 0.008 0.992 0.000 0.000
#&gt; SRR1617458     3  0.3569      0.806 0.000 0.000 0.804 0.196
#&gt; SRR1617459     3  0.3569      0.806 0.000 0.000 0.804 0.196
#&gt; SRR1617460     3  0.0336      0.920 0.008 0.000 0.992 0.000
#&gt; SRR1617461     3  0.0336      0.920 0.008 0.000 0.992 0.000
#&gt; SRR1617463     1  0.0000      0.938 1.000 0.000 0.000 0.000
#&gt; SRR1617462     1  0.0000      0.938 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4 p5
#&gt; SRR1617430     4   0.000      0.892 0.000 0.000 0.000 1.000  0
#&gt; SRR1617431     4   0.000      0.892 0.000 0.000 0.000 1.000  0
#&gt; SRR1617410     1   0.368      0.661 0.720 0.000 0.280 0.000  0
#&gt; SRR1617411     1   0.368      0.661 0.720 0.000 0.280 0.000  0
#&gt; SRR1617412     1   0.368      0.759 0.720 0.280 0.000 0.000  0
#&gt; SRR1617413     1   0.368      0.759 0.720 0.280 0.000 0.000  0
#&gt; SRR1617414     1   0.467      0.703 0.736 0.100 0.000 0.164  0
#&gt; SRR1617415     1   0.473      0.696 0.728 0.096 0.000 0.176  0
#&gt; SRR1617416     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1617417     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1617418     1   0.368      0.661 0.720 0.000 0.280 0.000  0
#&gt; SRR1617419     1   0.371      0.658 0.716 0.000 0.284 0.000  0
#&gt; SRR1617420     1   0.000      0.787 1.000 0.000 0.000 0.000  0
#&gt; SRR1617421     1   0.029      0.788 0.992 0.008 0.000 0.000  0
#&gt; SRR1617422     1   0.000      0.787 1.000 0.000 0.000 0.000  0
#&gt; SRR1617423     1   0.000      0.787 1.000 0.000 0.000 0.000  0
#&gt; SRR1617424     1   0.000      0.787 1.000 0.000 0.000 0.000  0
#&gt; SRR1617425     1   0.000      0.787 1.000 0.000 0.000 0.000  0
#&gt; SRR1617427     1   0.634      0.522 0.508 0.188 0.000 0.304  0
#&gt; SRR1617426     1   0.634      0.522 0.508 0.188 0.000 0.304  0
#&gt; SRR1617428     4   0.000      0.892 0.000 0.000 0.000 1.000  0
#&gt; SRR1617429     4   0.000      0.892 0.000 0.000 0.000 1.000  0
#&gt; SRR1617432     1   0.000      0.787 1.000 0.000 0.000 0.000  0
#&gt; SRR1617433     1   0.000      0.787 1.000 0.000 0.000 0.000  0
#&gt; SRR1617434     3   0.000      0.866 0.000 0.000 1.000 0.000  0
#&gt; SRR1617436     1   0.375      0.755 0.708 0.292 0.000 0.000  0
#&gt; SRR1617435     3   0.000      0.866 0.000 0.000 1.000 0.000  0
#&gt; SRR1617437     1   0.375      0.755 0.708 0.292 0.000 0.000  0
#&gt; SRR1617438     1   0.324      0.711 0.784 0.000 0.216 0.000  0
#&gt; SRR1617439     1   0.307      0.723 0.804 0.000 0.196 0.000  0
#&gt; SRR1617440     3   0.000      0.866 0.000 0.000 1.000 0.000  0
#&gt; SRR1617441     3   0.000      0.866 0.000 0.000 1.000 0.000  0
#&gt; SRR1617443     3   0.000      0.866 0.000 0.000 1.000 0.000  0
#&gt; SRR1617442     3   0.000      0.866 0.000 0.000 1.000 0.000  0
#&gt; SRR1617444     3   0.000      0.866 0.000 0.000 1.000 0.000  0
#&gt; SRR1617445     3   0.000      0.866 0.000 0.000 1.000 0.000  0
#&gt; SRR1617446     1   0.417      0.713 0.604 0.396 0.000 0.000  0
#&gt; SRR1617447     1   0.417      0.713 0.604 0.396 0.000 0.000  0
#&gt; SRR1617448     1   0.417      0.713 0.604 0.396 0.000 0.000  0
#&gt; SRR1617449     1   0.417      0.713 0.604 0.396 0.000 0.000  0
#&gt; SRR1617451     4   0.000      0.892 0.000 0.000 0.000 1.000  0
#&gt; SRR1617450     4   0.000      0.892 0.000 0.000 0.000 1.000  0
#&gt; SRR1617452     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1617454     4   0.368      0.658 0.280 0.000 0.000 0.720  0
#&gt; SRR1617453     5   0.000      1.000 0.000 0.000 0.000 0.000  1
#&gt; SRR1617456     2   0.417      1.000 0.000 0.604 0.396 0.000  0
#&gt; SRR1617457     2   0.417      1.000 0.000 0.604 0.396 0.000  0
#&gt; SRR1617455     4   0.368      0.658 0.280 0.000 0.000 0.720  0
#&gt; SRR1617458     2   0.417      1.000 0.000 0.604 0.396 0.000  0
#&gt; SRR1617459     2   0.417      1.000 0.000 0.604 0.396 0.000  0
#&gt; SRR1617460     3   0.364      0.468 0.272 0.000 0.728 0.000  0
#&gt; SRR1617461     3   0.327      0.545 0.220 0.000 0.780 0.000  0
#&gt; SRR1617463     1   0.000      0.787 1.000 0.000 0.000 0.000  0
#&gt; SRR1617462     1   0.000      0.787 1.000 0.000 0.000 0.000  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3 p4    p5 p6
#&gt; SRR1617430     2  0.0000      0.869 0.000 1.000 0.000  0 0.000  0
#&gt; SRR1617431     2  0.0000      0.869 0.000 1.000 0.000  0 0.000  0
#&gt; SRR1617410     3  0.3563      0.548 0.000 0.000 0.664  0 0.336  0
#&gt; SRR1617411     3  0.3563      0.548 0.000 0.000 0.664  0 0.336  0
#&gt; SRR1617412     3  0.3563      0.418 0.336 0.000 0.664  0 0.000  0
#&gt; SRR1617413     3  0.3563      0.418 0.336 0.000 0.664  0 0.000  0
#&gt; SRR1617414     1  0.5298      0.315 0.592 0.160 0.248  0 0.000  0
#&gt; SRR1617415     1  0.5344      0.310 0.588 0.172 0.240  0 0.000  0
#&gt; SRR1617416     4  0.0000      1.000 0.000 0.000 0.000  1 0.000  0
#&gt; SRR1617417     4  0.0000      1.000 0.000 0.000 0.000  1 0.000  0
#&gt; SRR1617418     3  0.3563      0.548 0.000 0.000 0.664  0 0.336  0
#&gt; SRR1617419     3  0.3578      0.544 0.000 0.000 0.660  0 0.340  0
#&gt; SRR1617420     3  0.0000      0.661 0.000 0.000 1.000  0 0.000  0
#&gt; SRR1617421     3  0.0363      0.658 0.012 0.000 0.988  0 0.000  0
#&gt; SRR1617422     3  0.0000      0.661 0.000 0.000 1.000  0 0.000  0
#&gt; SRR1617423     3  0.0000      0.661 0.000 0.000 1.000  0 0.000  0
#&gt; SRR1617424     3  0.0000      0.661 0.000 0.000 1.000  0 0.000  0
#&gt; SRR1617425     3  0.0000      0.661 0.000 0.000 1.000  0 0.000  0
#&gt; SRR1617427     1  0.3076      0.332 0.760 0.240 0.000  0 0.000  0
#&gt; SRR1617426     1  0.3050      0.336 0.764 0.236 0.000  0 0.000  0
#&gt; SRR1617428     2  0.0000      0.869 0.000 1.000 0.000  0 0.000  0
#&gt; SRR1617429     2  0.0000      0.869 0.000 1.000 0.000  0 0.000  0
#&gt; SRR1617432     3  0.3672      0.133 0.368 0.000 0.632  0 0.000  0
#&gt; SRR1617433     3  0.3672      0.133 0.368 0.000 0.632  0 0.000  0
#&gt; SRR1617434     5  0.0000      0.905 0.000 0.000 0.000  0 1.000  0
#&gt; SRR1617436     3  0.3607      0.399 0.348 0.000 0.652  0 0.000  0
#&gt; SRR1617435     5  0.0000      0.905 0.000 0.000 0.000  0 1.000  0
#&gt; SRR1617437     3  0.3607      0.399 0.348 0.000 0.652  0 0.000  0
#&gt; SRR1617438     3  0.3244      0.591 0.000 0.000 0.732  0 0.268  0
#&gt; SRR1617439     3  0.3126      0.601 0.000 0.000 0.752  0 0.248  0
#&gt; SRR1617440     5  0.0000      0.905 0.000 0.000 0.000  0 1.000  0
#&gt; SRR1617441     5  0.0000      0.905 0.000 0.000 0.000  0 1.000  0
#&gt; SRR1617443     5  0.0000      0.905 0.000 0.000 0.000  0 1.000  0
#&gt; SRR1617442     5  0.0000      0.905 0.000 0.000 0.000  0 1.000  0
#&gt; SRR1617444     5  0.0000      0.905 0.000 0.000 0.000  0 1.000  0
#&gt; SRR1617445     5  0.0000      0.905 0.000 0.000 0.000  0 1.000  0
#&gt; SRR1617446     1  0.3672      0.362 0.632 0.000 0.368  0 0.000  0
#&gt; SRR1617447     1  0.3672      0.362 0.632 0.000 0.368  0 0.000  0
#&gt; SRR1617448     1  0.3672      0.362 0.632 0.000 0.368  0 0.000  0
#&gt; SRR1617449     1  0.3672      0.362 0.632 0.000 0.368  0 0.000  0
#&gt; SRR1617451     2  0.0000      0.869 0.000 1.000 0.000  0 0.000  0
#&gt; SRR1617450     2  0.0000      0.869 0.000 1.000 0.000  0 0.000  0
#&gt; SRR1617452     4  0.0000      1.000 0.000 0.000 0.000  1 0.000  0
#&gt; SRR1617454     2  0.3563      0.565 0.000 0.664 0.336  0 0.000  0
#&gt; SRR1617453     4  0.0000      1.000 0.000 0.000 0.000  1 0.000  0
#&gt; SRR1617456     6  0.0000      1.000 0.000 0.000 0.000  0 0.000  1
#&gt; SRR1617457     6  0.0000      1.000 0.000 0.000 0.000  0 0.000  1
#&gt; SRR1617455     2  0.3563      0.565 0.000 0.664 0.336  0 0.000  0
#&gt; SRR1617458     6  0.0000      1.000 0.000 0.000 0.000  0 0.000  1
#&gt; SRR1617459     6  0.0000      1.000 0.000 0.000 0.000  0 0.000  1
#&gt; SRR1617460     5  0.3531      0.535 0.000 0.000 0.328  0 0.672  0
#&gt; SRR1617461     5  0.3288      0.606 0.000 0.000 0.276  0 0.724  0
#&gt; SRR1617463     3  0.0000      0.661 0.000 0.000 1.000  0 0.000  0
#&gt; SRR1617462     3  0.0000      0.661 0.000 0.000 1.000  0 0.000  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.377           0.685       0.872         0.4834 0.508   0.508
#> 3 3 0.659           0.800       0.868         0.3270 0.712   0.489
#> 4 4 0.449           0.449       0.647         0.1088 0.939   0.826
#> 5 5 0.560           0.505       0.642         0.0936 0.883   0.661
#> 6 6 0.704           0.640       0.753         0.0394 0.939   0.768
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.0672    0.82776 0.008 0.992
#&gt; SRR1617431     2  0.0672    0.82776 0.008 0.992
#&gt; SRR1617410     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617411     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617412     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617413     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617414     2  0.9896   -0.02673 0.440 0.560
#&gt; SRR1617415     2  0.9896   -0.02673 0.440 0.560
#&gt; SRR1617416     1  0.8555    0.52552 0.720 0.280
#&gt; SRR1617417     1  0.9087    0.45438 0.676 0.324
#&gt; SRR1617418     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617419     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617420     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617421     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617422     2  0.0938    0.83037 0.012 0.988
#&gt; SRR1617423     2  0.0938    0.83037 0.012 0.988
#&gt; SRR1617424     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617425     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617427     1  0.5946    0.70258 0.856 0.144
#&gt; SRR1617426     1  0.5946    0.70258 0.856 0.144
#&gt; SRR1617428     2  0.0000    0.82757 0.000 1.000
#&gt; SRR1617429     2  0.0000    0.82757 0.000 1.000
#&gt; SRR1617432     1  0.9686    0.27149 0.604 0.396
#&gt; SRR1617433     1  0.9686    0.27149 0.604 0.396
#&gt; SRR1617434     1  0.8955    0.46454 0.688 0.312
#&gt; SRR1617436     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617435     1  0.9686    0.27149 0.604 0.396
#&gt; SRR1617437     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617438     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617439     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617440     1  0.9970    0.01732 0.532 0.468
#&gt; SRR1617441     1  0.9983   -0.00753 0.524 0.476
#&gt; SRR1617443     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617442     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617444     1  0.9963    0.02889 0.536 0.464
#&gt; SRR1617445     1  0.9963    0.02889 0.536 0.464
#&gt; SRR1617446     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617447     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617448     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617449     1  0.0000    0.82226 1.000 0.000
#&gt; SRR1617451     2  0.0672    0.83234 0.008 0.992
#&gt; SRR1617450     2  0.0672    0.83234 0.008 0.992
#&gt; SRR1617452     2  0.6438    0.82976 0.164 0.836
#&gt; SRR1617454     2  0.5737    0.85870 0.136 0.864
#&gt; SRR1617453     2  0.6438    0.82976 0.164 0.836
#&gt; SRR1617456     2  0.5737    0.85870 0.136 0.864
#&gt; SRR1617457     2  0.5737    0.85870 0.136 0.864
#&gt; SRR1617455     2  0.5737    0.85870 0.136 0.864
#&gt; SRR1617458     2  0.5737    0.85870 0.136 0.864
#&gt; SRR1617459     2  0.5737    0.85870 0.136 0.864
#&gt; SRR1617460     2  0.5737    0.85870 0.136 0.864
#&gt; SRR1617461     2  0.5737    0.85870 0.136 0.864
#&gt; SRR1617463     2  0.5737    0.85870 0.136 0.864
#&gt; SRR1617462     2  0.5737    0.85870 0.136 0.864
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.0475      0.809 0.004 0.992 0.004
#&gt; SRR1617431     2  0.0475      0.809 0.004 0.992 0.004
#&gt; SRR1617410     3  0.0424      0.974 0.008 0.000 0.992
#&gt; SRR1617411     3  0.0424      0.974 0.008 0.000 0.992
#&gt; SRR1617412     3  0.0475      0.976 0.004 0.004 0.992
#&gt; SRR1617413     3  0.0475      0.976 0.004 0.004 0.992
#&gt; SRR1617414     2  0.4121      0.864 0.168 0.832 0.000
#&gt; SRR1617415     2  0.4121      0.864 0.168 0.832 0.000
#&gt; SRR1617416     1  0.1905      0.636 0.956 0.028 0.016
#&gt; SRR1617417     1  0.1905      0.636 0.956 0.028 0.016
#&gt; SRR1617418     3  0.0475      0.975 0.004 0.004 0.992
#&gt; SRR1617419     3  0.0475      0.975 0.004 0.004 0.992
#&gt; SRR1617420     3  0.0000      0.976 0.000 0.000 1.000
#&gt; SRR1617421     3  0.0000      0.976 0.000 0.000 1.000
#&gt; SRR1617422     2  0.3377      0.831 0.092 0.896 0.012
#&gt; SRR1617423     2  0.3213      0.834 0.092 0.900 0.008
#&gt; SRR1617424     3  0.0475      0.975 0.004 0.004 0.992
#&gt; SRR1617425     3  0.0475      0.975 0.004 0.004 0.992
#&gt; SRR1617427     3  0.4807      0.810 0.060 0.092 0.848
#&gt; SRR1617426     3  0.4709      0.816 0.056 0.092 0.852
#&gt; SRR1617428     2  0.3272      0.848 0.104 0.892 0.004
#&gt; SRR1617429     2  0.3272      0.848 0.104 0.892 0.004
#&gt; SRR1617432     2  0.6597      0.823 0.268 0.696 0.036
#&gt; SRR1617433     2  0.6252      0.833 0.268 0.708 0.024
#&gt; SRR1617434     1  0.7292      0.165 0.500 0.028 0.472
#&gt; SRR1617436     3  0.0475      0.975 0.004 0.004 0.992
#&gt; SRR1617435     1  0.7292      0.165 0.500 0.028 0.472
#&gt; SRR1617437     3  0.0475      0.975 0.004 0.004 0.992
#&gt; SRR1617438     3  0.0475      0.975 0.004 0.004 0.992
#&gt; SRR1617439     3  0.0475      0.975 0.004 0.004 0.992
#&gt; SRR1617440     1  0.6483      0.470 0.600 0.008 0.392
#&gt; SRR1617441     1  0.6483      0.470 0.600 0.008 0.392
#&gt; SRR1617443     3  0.0661      0.973 0.004 0.008 0.988
#&gt; SRR1617442     3  0.0661      0.973 0.004 0.008 0.988
#&gt; SRR1617444     1  0.6584      0.483 0.608 0.012 0.380
#&gt; SRR1617445     1  0.6566      0.488 0.612 0.012 0.376
#&gt; SRR1617446     3  0.0237      0.975 0.000 0.004 0.996
#&gt; SRR1617447     3  0.0237      0.975 0.000 0.004 0.996
#&gt; SRR1617448     3  0.0237      0.975 0.000 0.004 0.996
#&gt; SRR1617449     3  0.0424      0.975 0.000 0.008 0.992
#&gt; SRR1617451     2  0.2945      0.845 0.088 0.908 0.004
#&gt; SRR1617450     2  0.2945      0.845 0.088 0.908 0.004
#&gt; SRR1617452     1  0.1877      0.632 0.956 0.032 0.012
#&gt; SRR1617454     2  0.4682      0.814 0.192 0.804 0.004
#&gt; SRR1617453     1  0.1877      0.632 0.956 0.032 0.012
#&gt; SRR1617456     1  0.4834      0.557 0.792 0.204 0.004
#&gt; SRR1617457     1  0.5070      0.533 0.772 0.224 0.004
#&gt; SRR1617455     2  0.4682      0.814 0.192 0.804 0.004
#&gt; SRR1617458     1  0.3573      0.610 0.876 0.120 0.004
#&gt; SRR1617459     1  0.3573      0.610 0.876 0.120 0.004
#&gt; SRR1617460     2  0.5928      0.818 0.296 0.696 0.008
#&gt; SRR1617461     2  0.5928      0.818 0.296 0.696 0.008
#&gt; SRR1617463     2  0.5327      0.835 0.272 0.728 0.000
#&gt; SRR1617462     2  0.5327      0.835 0.272 0.728 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.4591     0.3633 0.084 0.800 0.116 0.000
#&gt; SRR1617431     2  0.4591     0.3633 0.084 0.800 0.116 0.000
#&gt; SRR1617410     1  0.6336     0.0932 0.480 0.000 0.060 0.460
#&gt; SRR1617411     1  0.6276     0.0942 0.480 0.000 0.056 0.464
#&gt; SRR1617412     1  0.3668     0.6667 0.808 0.000 0.188 0.004
#&gt; SRR1617413     1  0.3668     0.6667 0.808 0.000 0.188 0.004
#&gt; SRR1617414     2  0.7130     0.2271 0.020 0.536 0.360 0.084
#&gt; SRR1617415     2  0.7130     0.2271 0.020 0.536 0.360 0.084
#&gt; SRR1617416     4  0.4419     0.5742 0.072 0.012 0.088 0.828
#&gt; SRR1617417     4  0.4419     0.5742 0.072 0.012 0.088 0.828
#&gt; SRR1617418     1  0.8500     0.4196 0.472 0.044 0.236 0.248
#&gt; SRR1617419     1  0.8500     0.4196 0.472 0.044 0.236 0.248
#&gt; SRR1617420     1  0.1510     0.6812 0.956 0.000 0.016 0.028
#&gt; SRR1617421     1  0.1510     0.6812 0.956 0.000 0.016 0.028
#&gt; SRR1617422     2  0.7510     0.3199 0.044 0.556 0.312 0.088
#&gt; SRR1617423     2  0.7357     0.3241 0.036 0.564 0.312 0.088
#&gt; SRR1617424     1  0.2706     0.6713 0.900 0.000 0.020 0.080
#&gt; SRR1617425     1  0.2775     0.6703 0.896 0.000 0.020 0.084
#&gt; SRR1617427     1  0.6311     0.5075 0.708 0.180 0.048 0.064
#&gt; SRR1617426     1  0.6311     0.5075 0.708 0.180 0.048 0.064
#&gt; SRR1617428     2  0.6567     0.3395 0.004 0.592 0.316 0.088
#&gt; SRR1617429     2  0.6567     0.3395 0.004 0.592 0.316 0.088
#&gt; SRR1617432     3  0.7649     1.0000 0.004 0.204 0.484 0.308
#&gt; SRR1617433     3  0.7649     1.0000 0.004 0.204 0.484 0.308
#&gt; SRR1617434     4  0.5250     0.5537 0.072 0.040 0.096 0.792
#&gt; SRR1617436     1  0.5203     0.6464 0.720 0.000 0.232 0.048
#&gt; SRR1617435     4  0.5335     0.5497 0.072 0.044 0.096 0.788
#&gt; SRR1617437     1  0.5203     0.6464 0.720 0.000 0.232 0.048
#&gt; SRR1617438     1  0.7434     0.4716 0.512 0.000 0.232 0.256
#&gt; SRR1617439     1  0.7434     0.4716 0.512 0.000 0.232 0.256
#&gt; SRR1617440     4  0.6122     0.4043 0.276 0.008 0.064 0.652
#&gt; SRR1617441     4  0.6355     0.4383 0.260 0.020 0.064 0.656
#&gt; SRR1617443     1  0.6315     0.1601 0.508 0.000 0.060 0.432
#&gt; SRR1617442     1  0.6319     0.1532 0.504 0.000 0.060 0.436
#&gt; SRR1617444     4  0.4442     0.5725 0.236 0.004 0.008 0.752
#&gt; SRR1617445     4  0.4464     0.5858 0.224 0.004 0.012 0.760
#&gt; SRR1617446     1  0.1733     0.6837 0.948 0.000 0.028 0.024
#&gt; SRR1617447     1  0.1733     0.6837 0.948 0.000 0.028 0.024
#&gt; SRR1617448     1  0.0524     0.6834 0.988 0.000 0.008 0.004
#&gt; SRR1617449     1  0.0524     0.6834 0.988 0.000 0.008 0.004
#&gt; SRR1617451     2  0.2297     0.4229 0.032 0.932 0.012 0.024
#&gt; SRR1617450     2  0.2297     0.4229 0.032 0.932 0.012 0.024
#&gt; SRR1617452     4  0.3414     0.5837 0.004 0.072 0.048 0.876
#&gt; SRR1617454     2  0.3637     0.4006 0.004 0.864 0.052 0.080
#&gt; SRR1617453     4  0.3414     0.5837 0.004 0.072 0.048 0.876
#&gt; SRR1617456     2  0.7822    -0.0811 0.000 0.380 0.256 0.364
#&gt; SRR1617457     2  0.7822    -0.0811 0.000 0.380 0.256 0.364
#&gt; SRR1617455     2  0.3292     0.3976 0.004 0.880 0.036 0.080
#&gt; SRR1617458     4  0.7512     0.3187 0.004 0.336 0.172 0.488
#&gt; SRR1617459     4  0.7512     0.3187 0.004 0.336 0.172 0.488
#&gt; SRR1617460     2  0.8246     0.0512 0.024 0.400 0.196 0.380
#&gt; SRR1617461     2  0.8162     0.0541 0.020 0.404 0.196 0.380
#&gt; SRR1617463     2  0.6374     0.3614 0.000 0.644 0.228 0.128
#&gt; SRR1617462     2  0.6403     0.3632 0.000 0.640 0.232 0.128
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     2  0.5715     0.5557 0.048 0.676 0.000 0.208 0.068
#&gt; SRR1617431     2  0.5715     0.5557 0.048 0.676 0.000 0.208 0.068
#&gt; SRR1617410     1  0.6536     0.3344 0.632 0.016 0.200 0.112 0.040
#&gt; SRR1617411     1  0.6536     0.3344 0.632 0.016 0.200 0.112 0.040
#&gt; SRR1617412     3  0.1087     0.4770 0.016 0.000 0.968 0.008 0.008
#&gt; SRR1617413     3  0.1087     0.4770 0.016 0.000 0.968 0.008 0.008
#&gt; SRR1617414     2  0.3343     0.3087 0.016 0.812 0.000 0.172 0.000
#&gt; SRR1617415     2  0.3343     0.3087 0.016 0.812 0.000 0.172 0.000
#&gt; SRR1617416     1  0.5061     0.4532 0.624 0.004 0.012 0.340 0.020
#&gt; SRR1617417     1  0.4842     0.4482 0.632 0.004 0.004 0.340 0.020
#&gt; SRR1617418     3  0.7327     0.0878 0.136 0.004 0.544 0.092 0.224
#&gt; SRR1617419     3  0.7327     0.0878 0.136 0.004 0.544 0.092 0.224
#&gt; SRR1617420     3  0.6395     0.5474 0.356 0.008 0.528 0.092 0.016
#&gt; SRR1617421     3  0.6395     0.5474 0.356 0.008 0.528 0.092 0.016
#&gt; SRR1617422     2  0.1894     0.5205 0.072 0.920 0.000 0.008 0.000
#&gt; SRR1617423     2  0.1764     0.5278 0.064 0.928 0.000 0.008 0.000
#&gt; SRR1617424     3  0.6289     0.5437 0.400 0.008 0.492 0.092 0.008
#&gt; SRR1617425     3  0.6289     0.5437 0.400 0.008 0.492 0.092 0.008
#&gt; SRR1617427     3  0.7022     0.4795 0.384 0.100 0.464 0.044 0.008
#&gt; SRR1617426     3  0.7022     0.4795 0.384 0.100 0.464 0.044 0.008
#&gt; SRR1617428     2  0.0671     0.5516 0.016 0.980 0.000 0.004 0.000
#&gt; SRR1617429     2  0.0671     0.5516 0.016 0.980 0.000 0.004 0.000
#&gt; SRR1617432     4  0.6076     1.0000 0.076 0.432 0.000 0.476 0.016
#&gt; SRR1617433     4  0.6076     1.0000 0.076 0.432 0.000 0.476 0.016
#&gt; SRR1617434     1  0.6127     0.4313 0.528 0.008 0.072 0.380 0.012
#&gt; SRR1617436     3  0.1444     0.4637 0.012 0.000 0.948 0.040 0.000
#&gt; SRR1617435     1  0.6127     0.4313 0.528 0.008 0.072 0.380 0.012
#&gt; SRR1617437     3  0.1444     0.4637 0.012 0.000 0.948 0.040 0.000
#&gt; SRR1617438     3  0.6129     0.2017 0.124 0.000 0.668 0.068 0.140
#&gt; SRR1617439     3  0.6129     0.2017 0.124 0.000 0.668 0.068 0.140
#&gt; SRR1617440     1  0.7719     0.2877 0.476 0.004 0.172 0.088 0.260
#&gt; SRR1617441     1  0.7579     0.2984 0.492 0.004 0.164 0.080 0.260
#&gt; SRR1617443     1  0.4737     0.4320 0.680 0.000 0.284 0.012 0.024
#&gt; SRR1617442     1  0.4737     0.4320 0.680 0.000 0.284 0.012 0.024
#&gt; SRR1617444     1  0.4026     0.5603 0.832 0.024 0.036 0.016 0.092
#&gt; SRR1617445     1  0.4080     0.5594 0.828 0.024 0.036 0.016 0.096
#&gt; SRR1617446     3  0.6229     0.5511 0.384 0.000 0.500 0.104 0.012
#&gt; SRR1617447     3  0.6229     0.5511 0.384 0.000 0.500 0.104 0.012
#&gt; SRR1617448     3  0.6159     0.5540 0.380 0.004 0.508 0.104 0.004
#&gt; SRR1617449     3  0.6159     0.5540 0.380 0.004 0.508 0.104 0.004
#&gt; SRR1617451     2  0.5584     0.5787 0.000 0.584 0.000 0.324 0.092
#&gt; SRR1617450     2  0.5584     0.5787 0.000 0.584 0.000 0.324 0.092
#&gt; SRR1617452     1  0.6702     0.4569 0.588 0.008 0.028 0.200 0.176
#&gt; SRR1617454     2  0.6639     0.4878 0.004 0.476 0.000 0.304 0.216
#&gt; SRR1617453     1  0.6702     0.4569 0.588 0.008 0.028 0.200 0.176
#&gt; SRR1617456     5  0.3646     0.8718 0.052 0.100 0.000 0.012 0.836
#&gt; SRR1617457     5  0.3646     0.8718 0.052 0.100 0.000 0.012 0.836
#&gt; SRR1617455     2  0.6677     0.4751 0.004 0.468 0.000 0.304 0.224
#&gt; SRR1617458     5  0.2124     0.8715 0.096 0.004 0.000 0.000 0.900
#&gt; SRR1617459     5  0.2124     0.8715 0.096 0.004 0.000 0.000 0.900
#&gt; SRR1617460     2  0.6881     0.4320 0.168 0.604 0.004 0.144 0.080
#&gt; SRR1617461     2  0.6830     0.4366 0.168 0.608 0.004 0.144 0.076
#&gt; SRR1617463     2  0.4255     0.5858 0.068 0.800 0.000 0.112 0.020
#&gt; SRR1617462     2  0.4255     0.5858 0.068 0.800 0.000 0.112 0.020
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.3956      0.654 0.068 0.788 0.004 0.012 0.128 0.000
#&gt; SRR1617431     2  0.3956      0.654 0.068 0.788 0.004 0.012 0.128 0.000
#&gt; SRR1617410     4  0.7245      0.559 0.132 0.000 0.148 0.416 0.300 0.004
#&gt; SRR1617411     4  0.7245      0.559 0.132 0.000 0.148 0.416 0.300 0.004
#&gt; SRR1617412     1  0.4652      0.469 0.560 0.000 0.404 0.024 0.000 0.012
#&gt; SRR1617413     1  0.4652      0.469 0.560 0.000 0.404 0.024 0.000 0.012
#&gt; SRR1617414     2  0.4082      0.420 0.004 0.560 0.000 0.004 0.432 0.000
#&gt; SRR1617415     2  0.4082      0.420 0.004 0.560 0.000 0.004 0.432 0.000
#&gt; SRR1617416     4  0.5386      0.582 0.000 0.000 0.120 0.512 0.368 0.000
#&gt; SRR1617417     4  0.5386      0.582 0.000 0.000 0.120 0.512 0.368 0.000
#&gt; SRR1617418     3  0.1410      0.777 0.008 0.000 0.944 0.044 0.000 0.004
#&gt; SRR1617419     3  0.1410      0.777 0.008 0.000 0.944 0.044 0.000 0.004
#&gt; SRR1617420     1  0.1606      0.751 0.932 0.000 0.056 0.004 0.000 0.008
#&gt; SRR1617421     1  0.1542      0.752 0.936 0.000 0.052 0.004 0.000 0.008
#&gt; SRR1617422     2  0.4937      0.682 0.064 0.696 0.000 0.032 0.204 0.004
#&gt; SRR1617423     2  0.4867      0.684 0.064 0.700 0.000 0.028 0.204 0.004
#&gt; SRR1617424     1  0.1437      0.759 0.952 0.004 0.020 0.004 0.004 0.016
#&gt; SRR1617425     1  0.1348      0.759 0.956 0.004 0.016 0.004 0.004 0.016
#&gt; SRR1617427     1  0.5655      0.376 0.608 0.280 0.020 0.028 0.064 0.000
#&gt; SRR1617426     1  0.5655      0.376 0.608 0.280 0.020 0.028 0.064 0.000
#&gt; SRR1617428     2  0.3441      0.697 0.004 0.768 0.000 0.008 0.216 0.004
#&gt; SRR1617429     2  0.3441      0.697 0.004 0.768 0.000 0.008 0.216 0.004
#&gt; SRR1617432     5  0.2149      1.000 0.000 0.080 0.004 0.016 0.900 0.000
#&gt; SRR1617433     5  0.2149      1.000 0.000 0.080 0.004 0.016 0.900 0.000
#&gt; SRR1617434     4  0.6423      0.570 0.020 0.024 0.124 0.464 0.368 0.000
#&gt; SRR1617436     1  0.4580      0.419 0.528 0.000 0.440 0.028 0.004 0.000
#&gt; SRR1617435     4  0.6423      0.570 0.020 0.024 0.124 0.464 0.368 0.000
#&gt; SRR1617437     1  0.4580      0.419 0.528 0.000 0.440 0.028 0.004 0.000
#&gt; SRR1617438     3  0.1620      0.768 0.024 0.000 0.940 0.024 0.000 0.012
#&gt; SRR1617439     3  0.1620      0.768 0.024 0.000 0.940 0.024 0.000 0.012
#&gt; SRR1617440     3  0.5277      0.524 0.008 0.000 0.568 0.332 0.000 0.092
#&gt; SRR1617441     3  0.5265      0.531 0.008 0.000 0.572 0.328 0.000 0.092
#&gt; SRR1617443     4  0.4507      0.474 0.052 0.000 0.284 0.660 0.000 0.004
#&gt; SRR1617442     4  0.4507      0.474 0.052 0.000 0.284 0.660 0.000 0.004
#&gt; SRR1617444     4  0.4529      0.542 0.092 0.008 0.152 0.740 0.004 0.004
#&gt; SRR1617445     4  0.4529      0.542 0.092 0.008 0.152 0.740 0.004 0.004
#&gt; SRR1617446     1  0.0551      0.759 0.984 0.004 0.000 0.000 0.004 0.008
#&gt; SRR1617447     1  0.0551      0.759 0.984 0.004 0.000 0.000 0.004 0.008
#&gt; SRR1617448     1  0.0508      0.759 0.984 0.004 0.012 0.000 0.000 0.000
#&gt; SRR1617449     1  0.0508      0.759 0.984 0.004 0.012 0.000 0.000 0.000
#&gt; SRR1617451     2  0.1788      0.676 0.004 0.916 0.000 0.004 0.076 0.000
#&gt; SRR1617450     2  0.1788      0.676 0.004 0.916 0.000 0.004 0.076 0.000
#&gt; SRR1617452     4  0.3270      0.369 0.000 0.004 0.004 0.816 0.024 0.152
#&gt; SRR1617454     2  0.0893      0.700 0.004 0.972 0.004 0.004 0.000 0.016
#&gt; SRR1617453     4  0.3270      0.369 0.000 0.004 0.004 0.816 0.024 0.152
#&gt; SRR1617456     6  0.0291      0.996 0.000 0.004 0.000 0.004 0.000 0.992
#&gt; SRR1617457     6  0.0291      0.996 0.000 0.004 0.000 0.004 0.000 0.992
#&gt; SRR1617455     2  0.0893      0.700 0.004 0.972 0.004 0.004 0.000 0.016
#&gt; SRR1617458     6  0.0146      0.996 0.000 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1617459     6  0.0146      0.996 0.000 0.000 0.000 0.004 0.000 0.996
#&gt; SRR1617460     2  0.6579      0.305 0.012 0.532 0.132 0.276 0.040 0.008
#&gt; SRR1617461     2  0.6579      0.305 0.012 0.532 0.132 0.276 0.040 0.008
#&gt; SRR1617463     2  0.3456      0.703 0.004 0.800 0.004 0.028 0.164 0.000
#&gt; SRR1617462     2  0.3456      0.703 0.004 0.800 0.004 0.028 0.164 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:NMF






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 17713 rows and 54 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.813           0.914       0.962         0.4481 0.547   0.547
#> 3 3 0.592           0.781       0.884         0.4691 0.732   0.529
#> 4 4 0.573           0.636       0.797         0.1105 0.896   0.695
#> 5 5 0.545           0.560       0.748         0.0591 0.808   0.439
#> 6 6 0.656           0.576       0.750         0.0521 0.881   0.556
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1617430     2  0.7602      0.738 0.220 0.780
#&gt; SRR1617431     2  0.7674      0.733 0.224 0.776
#&gt; SRR1617410     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617411     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617412     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617413     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617414     1  0.1184      0.959 0.984 0.016
#&gt; SRR1617415     1  0.1184      0.959 0.984 0.016
#&gt; SRR1617416     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617417     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617418     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617419     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617420     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617421     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617422     1  0.5408      0.844 0.876 0.124
#&gt; SRR1617423     1  0.6531      0.788 0.832 0.168
#&gt; SRR1617424     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617425     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617427     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617426     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617428     2  0.9323      0.524 0.348 0.652
#&gt; SRR1617429     2  0.9087      0.573 0.324 0.676
#&gt; SRR1617432     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617433     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617434     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617436     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617435     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617437     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617438     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617439     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617440     1  0.7139      0.754 0.804 0.196
#&gt; SRR1617441     1  0.9775      0.294 0.588 0.412
#&gt; SRR1617443     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617442     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617444     1  0.0376      0.969 0.996 0.004
#&gt; SRR1617445     1  0.0376      0.969 0.996 0.004
#&gt; SRR1617446     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617447     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617448     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617449     1  0.0000      0.971 1.000 0.000
#&gt; SRR1617451     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617450     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617452     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617454     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617453     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617456     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617457     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617455     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617458     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617459     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617460     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617461     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617463     2  0.0000      0.931 0.000 1.000
#&gt; SRR1617462     2  0.0000      0.931 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1617430     2  0.5835      0.535 0.340 0.660 0.000
#&gt; SRR1617431     2  0.5882      0.526 0.348 0.652 0.000
#&gt; SRR1617410     1  0.5254      0.648 0.736 0.000 0.264
#&gt; SRR1617411     1  0.5431      0.614 0.716 0.000 0.284
#&gt; SRR1617412     3  0.0000      0.864 0.000 0.000 1.000
#&gt; SRR1617413     3  0.0237      0.865 0.004 0.000 0.996
#&gt; SRR1617414     1  0.3832      0.830 0.888 0.076 0.036
#&gt; SRR1617415     1  0.4092      0.824 0.876 0.088 0.036
#&gt; SRR1617416     1  0.1411      0.821 0.964 0.000 0.036
#&gt; SRR1617417     1  0.0237      0.832 0.996 0.000 0.004
#&gt; SRR1617418     3  0.1411      0.850 0.036 0.000 0.964
#&gt; SRR1617419     3  0.1411      0.850 0.036 0.000 0.964
#&gt; SRR1617420     3  0.2878      0.838 0.096 0.000 0.904
#&gt; SRR1617421     3  0.2959      0.836 0.100 0.000 0.900
#&gt; SRR1617422     1  0.5812      0.641 0.724 0.264 0.012
#&gt; SRR1617423     1  0.5986      0.608 0.704 0.284 0.012
#&gt; SRR1617424     1  0.5859      0.482 0.656 0.000 0.344
#&gt; SRR1617425     1  0.5291      0.639 0.732 0.000 0.268
#&gt; SRR1617427     1  0.4744      0.807 0.836 0.028 0.136
#&gt; SRR1617426     1  0.5223      0.776 0.800 0.024 0.176
#&gt; SRR1617428     1  0.2261      0.823 0.932 0.068 0.000
#&gt; SRR1617429     1  0.2356      0.821 0.928 0.072 0.000
#&gt; SRR1617432     1  0.1411      0.841 0.964 0.000 0.036
#&gt; SRR1617433     1  0.1411      0.841 0.964 0.000 0.036
#&gt; SRR1617434     1  0.1411      0.841 0.964 0.000 0.036
#&gt; SRR1617436     3  0.0892      0.867 0.020 0.000 0.980
#&gt; SRR1617435     1  0.1411      0.841 0.964 0.000 0.036
#&gt; SRR1617437     3  0.1163      0.866 0.028 0.000 0.972
#&gt; SRR1617438     3  0.0000      0.864 0.000 0.000 1.000
#&gt; SRR1617439     3  0.0000      0.864 0.000 0.000 1.000
#&gt; SRR1617440     3  0.1411      0.854 0.000 0.036 0.964
#&gt; SRR1617441     3  0.3686      0.785 0.000 0.140 0.860
#&gt; SRR1617443     3  0.1031      0.867 0.024 0.000 0.976
#&gt; SRR1617442     3  0.0424      0.866 0.008 0.000 0.992
#&gt; SRR1617444     3  0.4887      0.718 0.228 0.000 0.772
#&gt; SRR1617445     3  0.5541      0.679 0.252 0.008 0.740
#&gt; SRR1617446     3  0.5178      0.689 0.256 0.000 0.744
#&gt; SRR1617447     3  0.5785      0.554 0.332 0.000 0.668
#&gt; SRR1617448     3  0.5560      0.619 0.300 0.000 0.700
#&gt; SRR1617449     3  0.5706      0.581 0.320 0.000 0.680
#&gt; SRR1617451     2  0.0000      0.893 0.000 1.000 0.000
#&gt; SRR1617450     2  0.0000      0.893 0.000 1.000 0.000
#&gt; SRR1617452     2  0.0000      0.893 0.000 1.000 0.000
#&gt; SRR1617454     2  0.0000      0.893 0.000 1.000 0.000
#&gt; SRR1617453     2  0.0000      0.893 0.000 1.000 0.000
#&gt; SRR1617456     2  0.0000      0.893 0.000 1.000 0.000
#&gt; SRR1617457     2  0.0000      0.893 0.000 1.000 0.000
#&gt; SRR1617455     2  0.0000      0.893 0.000 1.000 0.000
#&gt; SRR1617458     2  0.0000      0.893 0.000 1.000 0.000
#&gt; SRR1617459     2  0.0000      0.893 0.000 1.000 0.000
#&gt; SRR1617460     2  0.5497      0.562 0.292 0.708 0.000
#&gt; SRR1617461     2  0.5397      0.584 0.280 0.720 0.000
#&gt; SRR1617463     2  0.2448      0.852 0.076 0.924 0.000
#&gt; SRR1617462     2  0.2356      0.853 0.072 0.928 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1617430     2  0.5297      0.640 0.292 0.676 0.000 0.032
#&gt; SRR1617431     2  0.5207      0.644 0.292 0.680 0.000 0.028
#&gt; SRR1617410     1  0.6352      0.644 0.656 0.000 0.188 0.156
#&gt; SRR1617411     1  0.6193      0.621 0.672 0.000 0.148 0.180
#&gt; SRR1617412     4  0.4989      0.211 0.000 0.000 0.472 0.528
#&gt; SRR1617413     4  0.4998      0.160 0.000 0.000 0.488 0.512
#&gt; SRR1617414     1  0.5478      0.521 0.540 0.016 0.444 0.000
#&gt; SRR1617415     1  0.5508      0.461 0.508 0.016 0.476 0.000
#&gt; SRR1617416     1  0.2704      0.574 0.876 0.000 0.000 0.124
#&gt; SRR1617417     1  0.2281      0.595 0.904 0.000 0.000 0.096
#&gt; SRR1617418     4  0.0707      0.690 0.000 0.000 0.020 0.980
#&gt; SRR1617419     4  0.0707      0.690 0.000 0.000 0.020 0.980
#&gt; SRR1617420     3  0.1302      0.724 0.000 0.000 0.956 0.044
#&gt; SRR1617421     3  0.0921      0.725 0.000 0.000 0.972 0.028
#&gt; SRR1617422     1  0.5067      0.680 0.736 0.048 0.216 0.000
#&gt; SRR1617423     1  0.5188      0.676 0.716 0.044 0.240 0.000
#&gt; SRR1617424     1  0.6903      0.348 0.508 0.000 0.112 0.380
#&gt; SRR1617425     1  0.6686      0.521 0.596 0.000 0.128 0.276
#&gt; SRR1617427     3  0.4695      0.386 0.252 0.012 0.732 0.004
#&gt; SRR1617426     3  0.4355      0.463 0.212 0.012 0.772 0.004
#&gt; SRR1617428     1  0.2342      0.620 0.912 0.080 0.000 0.008
#&gt; SRR1617429     1  0.2266      0.620 0.912 0.084 0.000 0.004
#&gt; SRR1617432     1  0.5244      0.589 0.600 0.012 0.388 0.000
#&gt; SRR1617433     1  0.5466      0.531 0.548 0.016 0.436 0.000
#&gt; SRR1617434     1  0.4632      0.649 0.688 0.004 0.308 0.000
#&gt; SRR1617436     3  0.2868      0.674 0.000 0.000 0.864 0.136
#&gt; SRR1617435     1  0.5212      0.558 0.572 0.008 0.420 0.000
#&gt; SRR1617437     3  0.2408      0.702 0.000 0.000 0.896 0.104
#&gt; SRR1617438     3  0.4994     -0.215 0.000 0.000 0.520 0.480
#&gt; SRR1617439     3  0.4907      0.014 0.000 0.000 0.580 0.420
#&gt; SRR1617440     4  0.6315      0.476 0.004 0.076 0.300 0.620
#&gt; SRR1617441     3  0.7083      0.206 0.004 0.164 0.580 0.252
#&gt; SRR1617443     3  0.2216      0.709 0.000 0.000 0.908 0.092
#&gt; SRR1617442     3  0.2530      0.696 0.000 0.000 0.888 0.112
#&gt; SRR1617444     3  0.0859      0.719 0.008 0.008 0.980 0.004
#&gt; SRR1617445     3  0.0992      0.718 0.008 0.012 0.976 0.004
#&gt; SRR1617446     4  0.1767      0.675 0.044 0.000 0.012 0.944
#&gt; SRR1617447     4  0.3471      0.611 0.072 0.000 0.060 0.868
#&gt; SRR1617448     3  0.5121      0.579 0.116 0.000 0.764 0.120
#&gt; SRR1617449     3  0.3877      0.613 0.112 0.000 0.840 0.048
#&gt; SRR1617451     2  0.1284      0.916 0.024 0.964 0.000 0.012
#&gt; SRR1617450     2  0.1284      0.916 0.024 0.964 0.000 0.012
#&gt; SRR1617452     2  0.1022      0.916 0.032 0.968 0.000 0.000
#&gt; SRR1617454     2  0.0188      0.915 0.004 0.996 0.000 0.000
#&gt; SRR1617453     2  0.0921      0.917 0.028 0.972 0.000 0.000
#&gt; SRR1617456     2  0.1484      0.905 0.004 0.960 0.016 0.020
#&gt; SRR1617457     2  0.1484      0.905 0.004 0.960 0.016 0.020
#&gt; SRR1617455     2  0.0188      0.915 0.004 0.996 0.000 0.000
#&gt; SRR1617458     2  0.1598      0.905 0.004 0.956 0.020 0.020
#&gt; SRR1617459     2  0.1598      0.905 0.004 0.956 0.020 0.020
#&gt; SRR1617460     2  0.4102      0.843 0.104 0.836 0.056 0.004
#&gt; SRR1617461     2  0.4193      0.837 0.100 0.832 0.064 0.004
#&gt; SRR1617463     2  0.1771      0.915 0.036 0.948 0.012 0.004
#&gt; SRR1617462     2  0.1545      0.916 0.040 0.952 0.008 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1617430     4  0.3063     0.6434 0.004 0.036 0.000 0.864 0.096
#&gt; SRR1617431     4  0.3063     0.6434 0.004 0.036 0.000 0.864 0.096
#&gt; SRR1617410     1  0.0960     0.6764 0.972 0.000 0.008 0.016 0.004
#&gt; SRR1617411     1  0.0833     0.6751 0.976 0.000 0.004 0.016 0.004
#&gt; SRR1617412     3  0.4101     0.5358 0.004 0.000 0.664 0.000 0.332
#&gt; SRR1617413     3  0.3969     0.5722 0.004 0.000 0.692 0.000 0.304
#&gt; SRR1617414     1  0.4915     0.6623 0.728 0.000 0.144 0.124 0.004
#&gt; SRR1617415     1  0.5347     0.6341 0.684 0.000 0.168 0.144 0.004
#&gt; SRR1617416     1  0.5029     0.4837 0.648 0.000 0.000 0.292 0.060
#&gt; SRR1617417     1  0.4777     0.4999 0.664 0.000 0.000 0.292 0.044
#&gt; SRR1617418     5  0.0404     0.7133 0.000 0.000 0.012 0.000 0.988
#&gt; SRR1617419     5  0.0404     0.7133 0.000 0.000 0.012 0.000 0.988
#&gt; SRR1617420     3  0.3001     0.7129 0.144 0.000 0.844 0.004 0.008
#&gt; SRR1617421     3  0.3129     0.7031 0.156 0.000 0.832 0.004 0.008
#&gt; SRR1617422     1  0.6845     0.4102 0.524 0.052 0.072 0.340 0.012
#&gt; SRR1617423     1  0.6891     0.3848 0.500 0.048 0.076 0.364 0.012
#&gt; SRR1617424     1  0.4892     0.5913 0.764 0.028 0.012 0.048 0.148
#&gt; SRR1617425     1  0.4612     0.6082 0.788 0.028 0.008 0.056 0.120
#&gt; SRR1617427     1  0.6528     0.1541 0.432 0.004 0.416 0.144 0.004
#&gt; SRR1617426     3  0.6525    -0.1878 0.408 0.004 0.440 0.144 0.004
#&gt; SRR1617428     4  0.1043     0.6301 0.040 0.000 0.000 0.960 0.000
#&gt; SRR1617429     4  0.1121     0.6283 0.044 0.000 0.000 0.956 0.000
#&gt; SRR1617432     1  0.3994     0.6811 0.792 0.000 0.140 0.068 0.000
#&gt; SRR1617433     1  0.4599     0.6685 0.752 0.000 0.156 0.088 0.004
#&gt; SRR1617434     1  0.3622     0.6862 0.820 0.000 0.124 0.056 0.000
#&gt; SRR1617436     3  0.0963     0.7525 0.000 0.000 0.964 0.000 0.036
#&gt; SRR1617435     1  0.4289     0.6590 0.760 0.000 0.176 0.064 0.000
#&gt; SRR1617437     3  0.0703     0.7552 0.000 0.000 0.976 0.000 0.024
#&gt; SRR1617438     3  0.3724     0.6401 0.000 0.020 0.776 0.000 0.204
#&gt; SRR1617439     3  0.3183     0.6870 0.000 0.016 0.828 0.000 0.156
#&gt; SRR1617440     5  0.6373     0.1445 0.000 0.416 0.164 0.000 0.420
#&gt; SRR1617441     2  0.5974     0.0802 0.000 0.568 0.284 0.000 0.148
#&gt; SRR1617443     3  0.0613     0.7558 0.004 0.000 0.984 0.004 0.008
#&gt; SRR1617442     3  0.0727     0.7561 0.004 0.000 0.980 0.004 0.012
#&gt; SRR1617444     3  0.4062     0.6967 0.148 0.024 0.804 0.012 0.012
#&gt; SRR1617445     3  0.4020     0.6988 0.144 0.024 0.808 0.012 0.012
#&gt; SRR1617446     1  0.5274     0.2366 0.564 0.012 0.012 0.012 0.400
#&gt; SRR1617447     1  0.5023     0.3423 0.620 0.012 0.008 0.012 0.348
#&gt; SRR1617448     1  0.4980     0.6266 0.752 0.044 0.164 0.012 0.028
#&gt; SRR1617449     1  0.4689     0.6217 0.756 0.048 0.176 0.012 0.008
#&gt; SRR1617451     4  0.3639     0.6860 0.000 0.144 0.000 0.812 0.044
#&gt; SRR1617450     4  0.3821     0.6845 0.000 0.148 0.000 0.800 0.052
#&gt; SRR1617452     4  0.4088     0.6091 0.000 0.368 0.000 0.632 0.000
#&gt; SRR1617454     4  0.4211     0.6157 0.000 0.360 0.000 0.636 0.004
#&gt; SRR1617453     4  0.4088     0.6091 0.000 0.368 0.000 0.632 0.000
#&gt; SRR1617456     2  0.0880     0.6747 0.000 0.968 0.000 0.032 0.000
#&gt; SRR1617457     2  0.0880     0.6747 0.000 0.968 0.000 0.032 0.000
#&gt; SRR1617455     4  0.4196     0.6186 0.000 0.356 0.000 0.640 0.004
#&gt; SRR1617458     2  0.0609     0.6744 0.000 0.980 0.000 0.020 0.000
#&gt; SRR1617459     2  0.0609     0.6744 0.000 0.980 0.000 0.020 0.000
#&gt; SRR1617460     2  0.6637     0.3784 0.224 0.568 0.020 0.184 0.004
#&gt; SRR1617461     2  0.6848     0.3341 0.244 0.532 0.020 0.200 0.004
#&gt; SRR1617463     4  0.6575     0.2198 0.152 0.400 0.004 0.440 0.004
#&gt; SRR1617462     4  0.6575     0.2221 0.152 0.400 0.004 0.440 0.004
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1617430     2  0.1700     0.7673 0.000 0.916 0.000 0.080 0.004 0.000
#&gt; SRR1617431     2  0.1644     0.7703 0.000 0.920 0.000 0.076 0.004 0.000
#&gt; SRR1617410     5  0.2418     0.8029 0.092 0.000 0.000 0.016 0.884 0.008
#&gt; SRR1617411     5  0.2648     0.7972 0.092 0.000 0.004 0.020 0.876 0.008
#&gt; SRR1617412     3  0.3384     0.6096 0.004 0.000 0.760 0.228 0.008 0.000
#&gt; SRR1617413     3  0.3056     0.6447 0.004 0.000 0.804 0.184 0.008 0.000
#&gt; SRR1617414     5  0.1555     0.8656 0.008 0.040 0.012 0.000 0.940 0.000
#&gt; SRR1617415     5  0.1821     0.8635 0.008 0.040 0.024 0.000 0.928 0.000
#&gt; SRR1617416     1  0.5653     0.2139 0.640 0.120 0.000 0.056 0.184 0.000
#&gt; SRR1617417     1  0.5596     0.2167 0.640 0.120 0.000 0.048 0.192 0.000
#&gt; SRR1617418     4  0.0603     1.0000 0.000 0.004 0.016 0.980 0.000 0.000
#&gt; SRR1617419     4  0.0603     1.0000 0.000 0.004 0.016 0.980 0.000 0.000
#&gt; SRR1617420     3  0.4033     0.2736 0.004 0.000 0.588 0.004 0.404 0.000
#&gt; SRR1617421     3  0.3944     0.2092 0.004 0.000 0.568 0.000 0.428 0.000
#&gt; SRR1617422     1  0.5928     0.3439 0.584 0.272 0.064 0.000 0.076 0.004
#&gt; SRR1617423     1  0.6312     0.2987 0.540 0.300 0.064 0.000 0.084 0.012
#&gt; SRR1617424     1  0.3212     0.3216 0.840 0.000 0.012 0.100 0.048 0.000
#&gt; SRR1617425     1  0.2648     0.3485 0.884 0.004 0.008 0.064 0.040 0.000
#&gt; SRR1617427     5  0.4679     0.6584 0.012 0.164 0.100 0.004 0.720 0.000
#&gt; SRR1617426     5  0.4807     0.6466 0.012 0.164 0.112 0.004 0.708 0.000
#&gt; SRR1617428     2  0.2520     0.7369 0.108 0.872 0.012 0.000 0.008 0.000
#&gt; SRR1617429     2  0.2473     0.7399 0.104 0.876 0.012 0.000 0.008 0.000
#&gt; SRR1617432     5  0.0508     0.8689 0.004 0.012 0.000 0.000 0.984 0.000
#&gt; SRR1617433     5  0.0748     0.8696 0.004 0.016 0.004 0.000 0.976 0.000
#&gt; SRR1617434     5  0.1237     0.8662 0.020 0.004 0.020 0.000 0.956 0.000
#&gt; SRR1617436     3  0.1219     0.7223 0.048 0.000 0.948 0.004 0.000 0.000
#&gt; SRR1617435     5  0.1116     0.8666 0.008 0.004 0.028 0.000 0.960 0.000
#&gt; SRR1617437     3  0.1141     0.7224 0.052 0.000 0.948 0.000 0.000 0.000
#&gt; SRR1617438     3  0.3253     0.7054 0.096 0.000 0.832 0.068 0.004 0.000
#&gt; SRR1617439     3  0.2968     0.7084 0.092 0.000 0.852 0.052 0.004 0.000
#&gt; SRR1617440     6  0.6747     0.2848 0.044 0.000 0.276 0.224 0.004 0.452
#&gt; SRR1617441     6  0.5640     0.2440 0.044 0.004 0.400 0.036 0.004 0.512
#&gt; SRR1617443     3  0.0937     0.7111 0.000 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1617442     3  0.1196     0.7103 0.000 0.000 0.952 0.008 0.040 0.000
#&gt; SRR1617444     3  0.4689     0.4426 0.340 0.004 0.612 0.004 0.040 0.000
#&gt; SRR1617445     3  0.4615     0.4436 0.340 0.004 0.612 0.000 0.044 0.000
#&gt; SRR1617446     1  0.6523     0.0973 0.356 0.000 0.008 0.348 0.280 0.008
#&gt; SRR1617447     1  0.6503     0.1934 0.380 0.000 0.008 0.272 0.332 0.008
#&gt; SRR1617448     1  0.5512     0.1785 0.504 0.000 0.020 0.032 0.420 0.024
#&gt; SRR1617449     1  0.5353     0.1766 0.512 0.000 0.016 0.016 0.420 0.036
#&gt; SRR1617451     2  0.0713     0.8127 0.000 0.972 0.000 0.000 0.000 0.028
#&gt; SRR1617450     2  0.0713     0.8127 0.000 0.972 0.000 0.000 0.000 0.028
#&gt; SRR1617452     2  0.2613     0.8049 0.012 0.848 0.000 0.000 0.000 0.140
#&gt; SRR1617454     2  0.2482     0.8019 0.004 0.848 0.000 0.000 0.000 0.148
#&gt; SRR1617453     2  0.2613     0.8049 0.012 0.848 0.000 0.000 0.000 0.140
#&gt; SRR1617456     6  0.0458     0.7340 0.000 0.016 0.000 0.000 0.000 0.984
#&gt; SRR1617457     6  0.0458     0.7340 0.000 0.016 0.000 0.000 0.000 0.984
#&gt; SRR1617455     2  0.2482     0.8019 0.004 0.848 0.000 0.000 0.000 0.148
#&gt; SRR1617458     6  0.0405     0.7338 0.004 0.008 0.000 0.000 0.000 0.988
#&gt; SRR1617459     6  0.0405     0.7338 0.004 0.008 0.000 0.000 0.000 0.988
#&gt; SRR1617460     1  0.7564     0.2727 0.388 0.240 0.056 0.000 0.040 0.276
#&gt; SRR1617461     1  0.7767     0.2728 0.376 0.240 0.068 0.000 0.048 0.268
#&gt; SRR1617463     2  0.6138    -0.0842 0.424 0.436 0.028 0.000 0.008 0.104
#&gt; SRR1617462     1  0.6004    -0.0640 0.444 0.432 0.028 0.000 0.008 0.088
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#>  [1] grid      parallel  stats4    stats     graphics  grDevices utils     datasets  methods  
#> [10] base     
#> 
#> other attached packages:
#>  [1] genefilter_1.66.0           ComplexHeatmap_2.1.1        markdown_1.1               
#>  [4] knitr_1.26                  cola_1.3.2                  SummarizedExperiment_1.14.1
#>  [7] DelayedArray_0.10.0         BiocParallel_1.18.1         matrixStats_0.55.0         
#> [10] Biobase_2.44.0              GenomicRanges_1.36.1        GenomeInfoDb_1.20.0        
#> [13] IRanges_2.18.3              S4Vectors_0.22.1            BiocGenerics_0.30.0        
#> [16] GetoptLong_0.1.7           
#> 
#> loaded via a namespace (and not attached):
#>  [1] bitops_1.0-6           bit64_0.9-7            doParallel_1.0.15      RColorBrewer_1.1-2    
#>  [5] httr_1.4.1             backports_1.1.5        tools_3.6.0            R6_2.4.1              
#>  [9] DBI_1.0.0              lazyeval_0.2.2         colorspace_1.4-1       withr_2.1.2           
#> [13] tidyselect_0.2.5       gridExtra_2.3          bit_1.1-14             compiler_3.6.0        
#> [17] xml2_1.2.2             microbenchmark_1.4-7   pkgmaker_0.28          slam_0.1-46           
#> [21] scales_1.1.0           NMF_0.23.6             stringr_1.4.0          digest_0.6.23         
#> [25] XVector_0.24.0         pkgconfig_2.0.3        bibtex_0.4.2           highr_0.8             
#> [29] rlang_0.4.2            GlobalOptions_0.1.1    RSQLite_2.1.2          impute_1.58.0         
#> [33] shape_1.4.4            mclust_5.4.5           dendextend_1.12.0      dplyr_0.8.3           
#> [37] RCurl_1.95-4.12        magrittr_1.5           GenomeInfoDbData_1.2.1 Matrix_1.2-17         
#> [41] Rcpp_1.0.3             munsell_0.5.0          viridis_0.5.1          lifecycle_0.1.0       
#> [45] stringi_1.4.3          zlibbioc_1.30.0        plyr_1.8.4             blob_1.2.0            
#> [49] crayon_1.3.4           lattice_0.20-38        splines_3.6.0          annotate_1.62.0       
#> [53] circlize_0.4.9         zeallot_0.1.0          pillar_1.4.2           rjson_0.2.20          
#> [57] rngtools_1.4           reshape2_1.4.3         codetools_0.2-16       XML_3.98-1.20         
#> [61] glue_1.3.1             evaluate_0.14          vctrs_0.2.0            png_0.1-7             
#> [65] foreach_1.4.7          polyclip_1.10-0        gtable_0.3.0           purrr_0.3.3           
#> [69] clue_0.3-57            assertthat_0.2.1       ggplot2_3.2.1          xfun_0.11             
#> [73] gridBase_0.4-7         eulerr_6.0.0           xtable_1.8-4           skmeans_0.2-11        
#> [77] survival_2.44-1.1      viridisLite_0.3.0      tibble_2.1.3           iterators_1.0.12      
#> [81] memoise_1.1.0          AnnotationDbi_1.46.1   registry_0.5-1         GTF_0.0.1             
#> [85] cluster_2.1.0          brew_1.0-6
```


