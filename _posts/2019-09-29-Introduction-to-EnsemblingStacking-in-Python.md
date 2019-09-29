---
title: "(KR-KaggleKernelTranscription)Introduction to EnsemblingStacking in Python"
tags: 
  - Kaggle competition
  - Machine Learning
  - KaggleTranscription(Korean)
categories:
  - PostTranslation
toc: true
comments: 
  provider: "disqus"
  disqus:
    shortname: "https-brstar96-github-io"
header:
  teaser: /assets/Images/kaggletitanic.png
---

<span style="font-size:11pt">This code is written by [Anisotropic](https://www.kaggle.com/arthurtok/introduction-to-ensembling-stacking-in-python).</span>


<html><head><meta charset="utf-8">

<title>02-Introduction_to_EnsemblingStacking_in_Python</title>

<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.1.10/require.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>



<style type="text/css">
    /*!
*
* Twitter Bootstrap
*
*/
/*!
 * Bootstrap v3.3.7 (http://getbootstrap.com)
 * Copyright 2011-2016 Twitter, Inc.
 * Licensed under MIT (https://github.com/twbs/bootstrap/blob/master/LICENSE)
 */
/*! normalize.css v3.0.3 | MIT License | github.com/necolas/normalize.css */
html {
  font-family: sans-serif;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}
body {
  margin: 0;
}
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
main,
menu,
nav,
section,
summary {
  display: block;
}
audio,
canvas,
progress,
video {
  display: inline-block;
  vertical-align: baseline;
}
audio:not([controls]) {
  display: none;
  height: 0;
}
[hidden],
template {
  display: none;
}
a {
  background-color: transparent;
}
a:active,
a:hover {
  outline: 0;
}
abbr[title] {
  border-bottom: 1px dotted;
}
b,
strong {
  font-weight: bold;
}
dfn {
  font-style: italic;
}
h1 {
  font-size: 2em;
  margin: 0.67em 0;
}
mark {
  background: #ff0;
  color: #000;
}
small {
  font-size: 80%;
}
sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
  vertical-align: baseline;
}
sup {
  top: -0.5em;
}
sub {
  bottom: -0.25em;
}
img {
  border: 0;
}
svg:not(:root) {
  overflow: hidden;
}
figure {
  margin: 1em 40px;
}
hr {
  box-sizing: content-box;
  height: 0;
}
pre {
  overflow: auto;
}
code,
kbd,
pre,
samp {
  font-family: monospace, monospace;
  font-size: 1em;
}
button,
input,
optgroup,
select,
textarea {
  color: inherit;
  font: inherit;
  margin: 0;
}
button {
  overflow: visible;
}
button,
select {
  text-transform: none;
}
button,
html input[type="button"],
input[type="reset"],
input[type="submit"] {
  -webkit-appearance: button;
  cursor: pointer;
}
button[disabled],
html input[disabled] {
  cursor: default;
}
button::-moz-focus-inner,
input::-moz-focus-inner {
  border: 0;
  padding: 0;
}
input {
  line-height: normal;
}
input[type="checkbox"],
input[type="radio"] {
  box-sizing: border-box;
  padding: 0;
}
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  height: auto;
}
input[type="search"] {
  -webkit-appearance: textfield;
  box-sizing: content-box;
}
input[type="search"]::-webkit-search-cancel-button,
input[type="search"]::-webkit-search-decoration {
  -webkit-appearance: none;
}
fieldset {
  border: 1px solid #c0c0c0;
  margin: 0 2px;
  padding: 0.35em 0.625em 0.75em;
}
legend {
  border: 0;
  padding: 0;
}
textarea {
  overflow: auto;
}
optgroup {
  font-weight: bold;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
td,
th {
  padding: 0;
}
/*! Source: https://github.com/h5bp/html5-boilerplate/blob/master/src/css/main.css */
@media print {
  *,
  *:before,
  *:after {
    background: transparent !important;
    box-shadow: none !important;
    text-shadow: none !important;
  }
  a,
  a:visited {
    text-decoration: underline;
  }
  a[href]:after {
    content: " (" attr(href) ")";
  }
  abbr[title]:after {
    content: " (" attr(title) ")";
  }
  a[href^="#"]:after,
  a[href^="javascript:"]:after {
    content: "";
  }
  pre,
  blockquote {
    border: 1px solid #999;
    page-break-inside: avoid;
  }
  thead {
    display: table-header-group;
  }
  tr,
  img {
    page-break-inside: avoid;
  }
  img {
    max-width: 100% !important;
  }
  p,
  h2,
  h3 {
    orphans: 3;
    widows: 3;
  }
  h2,
  h3 {
    page-break-after: avoid;
  }
  .navbar {
    display: none;
  }
  .btn > .caret,
  .dropup > .btn > .caret {
    border-top-color: #000 !important;
  }
  .label {
    border: 1px solid #000;
  }
  .table {
    border-collapse: collapse !important;
  }
  .table td,
  .table th {
    background-color: #fff !important;
  }
  .table-bordered th,
  .table-bordered td {
    border: 1px solid #ddd !important;
  }
}
@font-face {
  font-family: 'Glyphicons Halflings';
  src: url('../components/bootstrap/fonts/glyphicons-halflings-regular.eot');
  src: url('../components/bootstrap/fonts/glyphicons-halflings-regular.eot?#iefix') format('embedded-opentype'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.woff2') format('woff2'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.woff') format('woff'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.ttf') format('truetype'), url('../components/bootstrap/fonts/glyphicons-halflings-regular.svg#glyphicons_halflingsregular') format('svg');
}
.glyphicon {
  position: relative;
  top: 1px;
  display: inline-block;
  font-family: 'Glyphicons Halflings';
  font-style: normal;
  font-weight: normal;
  line-height: 1;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.glyphicon-asterisk:before {
  content: "\002a";
}
.glyphicon-plus:before {
  content: "\002b";
}
.glyphicon-euro:before,
.glyphicon-eur:before {
  content: "\20ac";
}
.glyphicon-minus:before {
  content: "\2212";
}
.glyphicon-cloud:before {
  content: "\2601";
}
.glyphicon-envelope:before {
  content: "\2709";
}
.glyphicon-pencil:before {
  content: "\270f";
}
.glyphicon-glass:before {
  content: "\e001";
}
.glyphicon-music:before {
  content: "\e002";
}
.glyphicon-search:before {
  content: "\e003";
}
.glyphicon-heart:before {
  content: "\e005";
}
.glyphicon-star:before {
  content: "\e006";
}
.glyphicon-star-empty:before {
  content: "\e007";
}
.glyphicon-user:before {
  content: "\e008";
}
.glyphicon-film:before {
  content: "\e009";
}
.glyphicon-th-large:before {
  content: "\e010";
}
.glyphicon-th:before {
  content: "\e011";
}
.glyphicon-th-list:before {
  content: "\e012";
}
.glyphicon-ok:before {
  content: "\e013";
}
.glyphicon-remove:before {
  content: "\e014";
}
.glyphicon-zoom-in:before {
  content: "\e015";
}
.glyphicon-zoom-out:before {
  content: "\e016";
}
.glyphicon-off:before {
  content: "\e017";
}
.glyphicon-signal:before {
  content: "\e018";
}
.glyphicon-cog:before {
  content: "\e019";
}
.glyphicon-trash:before {
  content: "\e020";
}
.glyphicon-home:before {
  content: "\e021";
}
.glyphicon-file:before {
  content: "\e022";
}
.glyphicon-time:before {
  content: "\e023";
}
.glyphicon-road:before {
  content: "\e024";
}
.glyphicon-download-alt:before {
  content: "\e025";
}
.glyphicon-download:before {
  content: "\e026";
}
.glyphicon-upload:before {
  content: "\e027";
}
.glyphicon-inbox:before {
  content: "\e028";
}
.glyphicon-play-circle:before {
  content: "\e029";
}
.glyphicon-repeat:before {
  content: "\e030";
}
.glyphicon-refresh:before {
  content: "\e031";
}
.glyphicon-list-alt:before {
  content: "\e032";
}
.glyphicon-lock:before {
  content: "\e033";
}
.glyphicon-flag:before {
  content: "\e034";
}
.glyphicon-headphones:before {
  content: "\e035";
}
.glyphicon-volume-off:before {
  content: "\e036";
}
.glyphicon-volume-down:before {
  content: "\e037";
}
.glyphicon-volume-up:before {
  content: "\e038";
}
.glyphicon-qrcode:before {
  content: "\e039";
}
.glyphicon-barcode:before {
  content: "\e040";
}
.glyphicon-tag:before {
  content: "\e041";
}
.glyphicon-tags:before {
  content: "\e042";
}
.glyphicon-book:before {
  content: "\e043";
}
.glyphicon-bookmark:before {
  content: "\e044";
}
.glyphicon-print:before {
  content: "\e045";
}
.glyphicon-camera:before {
  content: "\e046";
}
.glyphicon-font:before {
  content: "\e047";
}
.glyphicon-bold:before {
  content: "\e048";
}
.glyphicon-italic:before {
  content: "\e049";
}
.glyphicon-text-height:before {
  content: "\e050";
}
.glyphicon-text-width:before {
  content: "\e051";
}
.glyphicon-align-left:before {
  content: "\e052";
}
.glyphicon-align-center:before {
  content: "\e053";
}
.glyphicon-align-right:before {
  content: "\e054";
}
.glyphicon-align-justify:before {
  content: "\e055";
}
.glyphicon-list:before {
  content: "\e056";
}
.glyphicon-indent-left:before {
  content: "\e057";
}
.glyphicon-indent-right:before {
  content: "\e058";
}
.glyphicon-facetime-video:before {
  content: "\e059";
}
.glyphicon-picture:before {
  content: "\e060";
}
.glyphicon-map-marker:before {
  content: "\e062";
}
.glyphicon-adjust:before {
  content: "\e063";
}
.glyphicon-tint:before {
  content: "\e064";
}
.glyphicon-edit:before {
  content: "\e065";
}
.glyphicon-share:before {
  content: "\e066";
}
.glyphicon-check:before {
  content: "\e067";
}
.glyphicon-move:before {
  content: "\e068";
}
.glyphicon-step-backward:before {
  content: "\e069";
}
.glyphicon-fast-backward:before {
  content: "\e070";
}
.glyphicon-backward:before {
  content: "\e071";
}
.glyphicon-play:before {
  content: "\e072";
}
.glyphicon-pause:before {
  content: "\e073";
}
.glyphicon-stop:before {
  content: "\e074";
}
.glyphicon-forward:before {
  content: "\e075";
}
.glyphicon-fast-forward:before {
  content: "\e076";
}
.glyphicon-step-forward:before {
  content: "\e077";
}
.glyphicon-eject:before {
  content: "\e078";
}
.glyphicon-chevron-left:before {
  content: "\e079";
}
.glyphicon-chevron-right:before {
  content: "\e080";
}
.glyphicon-plus-sign:before {
  content: "\e081";
}
.glyphicon-minus-sign:before {
  content: "\e082";
}
.glyphicon-remove-sign:before {
  content: "\e083";
}
.glyphicon-ok-sign:before {
  content: "\e084";
}
.glyphicon-question-sign:before {
  content: "\e085";
}
.glyphicon-info-sign:before {
  content: "\e086";
}
.glyphicon-screenshot:before {
  content: "\e087";
}
.glyphicon-remove-circle:before {
  content: "\e088";
}
.glyphicon-ok-circle:before {
  content: "\e089";
}
.glyphicon-ban-circle:before {
  content: "\e090";
}
.glyphicon-arrow-left:before {
  content: "\e091";
}
.glyphicon-arrow-right:before {
  content: "\e092";
}
.glyphicon-arrow-up:before {
  content: "\e093";
}
.glyphicon-arrow-down:before {
  content: "\e094";
}
.glyphicon-share-alt:before {
  content: "\e095";
}
.glyphicon-resize-full:before {
  content: "\e096";
}
.glyphicon-resize-small:before {
  content: "\e097";
}
.glyphicon-exclamation-sign:before {
  content: "\e101";
}
.glyphicon-gift:before {
  content: "\e102";
}
.glyphicon-leaf:before {
  content: "\e103";
}
.glyphicon-fire:before {
  content: "\e104";
}
.glyphicon-eye-open:before {
  content: "\e105";
}
.glyphicon-eye-close:before {
  content: "\e106";
}
.glyphicon-warning-sign:before {
  content: "\e107";
}
.glyphicon-plane:before {
  content: "\e108";
}
.glyphicon-calendar:before {
  content: "\e109";
}
.glyphicon-random:before {
  content: "\e110";
}
.glyphicon-comment:before {
  content: "\e111";
}
.glyphicon-magnet:before {
  content: "\e112";
}
.glyphicon-chevron-up:before {
  content: "\e113";
}
.glyphicon-chevron-down:before {
  content: "\e114";
}
.glyphicon-retweet:before {
  content: "\e115";
}
.glyphicon-shopping-cart:before {
  content: "\e116";
}
.glyphicon-folder-close:before {
  content: "\e117";
}
.glyphicon-folder-open:before {
  content: "\e118";
}
.glyphicon-resize-vertical:before {
  content: "\e119";
}
.glyphicon-resize-horizontal:before {
  content: "\e120";
}
.glyphicon-hdd:before {
  content: "\e121";
}
.glyphicon-bullhorn:before {
  content: "\e122";
}
.glyphicon-bell:before {
  content: "\e123";
}
.glyphicon-certificate:before {
  content: "\e124";
}
.glyphicon-thumbs-up:before {
  content: "\e125";
}
.glyphicon-thumbs-down:before {
  content: "\e126";
}
.glyphicon-hand-right:before {
  content: "\e127";
}
.glyphicon-hand-left:before {
  content: "\e128";
}
.glyphicon-hand-up:before {
  content: "\e129";
}
.glyphicon-hand-down:before {
  content: "\e130";
}
.glyphicon-circle-arrow-right:before {
  content: "\e131";
}
.glyphicon-circle-arrow-left:before {
  content: "\e132";
}
.glyphicon-circle-arrow-up:before {
  content: "\e133";
}
.glyphicon-circle-arrow-down:before {
  content: "\e134";
}
.glyphicon-globe:before {
  content: "\e135";
}
.glyphicon-wrench:before {
  content: "\e136";
}
.glyphicon-tasks:before {
  content: "\e137";
}
.glyphicon-filter:before {
  content: "\e138";
}
.glyphicon-briefcase:before {
  content: "\e139";
}
.glyphicon-fullscreen:before {
  content: "\e140";
}
.glyphicon-dashboard:before {
  content: "\e141";
}
.glyphicon-paperclip:before {
  content: "\e142";
}
.glyphicon-heart-empty:before {
  content: "\e143";
}
.glyphicon-link:before {
  content: "\e144";
}
.glyphicon-phone:before {
  content: "\e145";
}
.glyphicon-pushpin:before {
  content: "\e146";
}
.glyphicon-usd:before {
  content: "\e148";
}
.glyphicon-gbp:before {
  content: "\e149";
}
.glyphicon-sort:before {
  content: "\e150";
}
.glyphicon-sort-by-alphabet:before {
  content: "\e151";
}
.glyphicon-sort-by-alphabet-alt:before {
  content: "\e152";
}
.glyphicon-sort-by-order:before {
  content: "\e153";
}
.glyphicon-sort-by-order-alt:before {
  content: "\e154";
}
.glyphicon-sort-by-attributes:before {
  content: "\e155";
}
.glyphicon-sort-by-attributes-alt:before {
  content: "\e156";
}
.glyphicon-unchecked:before {
  content: "\e157";
}
.glyphicon-expand:before {
  content: "\e158";
}
.glyphicon-collapse-down:before {
  content: "\e159";
}
.glyphicon-collapse-up:before {
  content: "\e160";
}
.glyphicon-log-in:before {
  content: "\e161";
}
.glyphicon-flash:before {
  content: "\e162";
}
.glyphicon-log-out:before {
  content: "\e163";
}
.glyphicon-new-window:before {
  content: "\e164";
}
.glyphicon-record:before {
  content: "\e165";
}
.glyphicon-save:before {
  content: "\e166";
}
.glyphicon-open:before {
  content: "\e167";
}
.glyphicon-saved:before {
  content: "\e168";
}
.glyphicon-import:before {
  content: "\e169";
}
.glyphicon-export:before {
  content: "\e170";
}
.glyphicon-send:before {
  content: "\e171";
}
.glyphicon-floppy-disk:before {
  content: "\e172";
}
.glyphicon-floppy-saved:before {
  content: "\e173";
}
.glyphicon-floppy-remove:before {
  content: "\e174";
}
.glyphicon-floppy-save:before {
  content: "\e175";
}
.glyphicon-floppy-open:before {
  content: "\e176";
}
.glyphicon-credit-card:before {
  content: "\e177";
}
.glyphicon-transfer:before {
  content: "\e178";
}
.glyphicon-cutlery:before {
  content: "\e179";
}
.glyphicon-header:before {
  content: "\e180";
}
.glyphicon-compressed:before {
  content: "\e181";
}
.glyphicon-earphone:before {
  content: "\e182";
}
.glyphicon-phone-alt:before {
  content: "\e183";
}
.glyphicon-tower:before {
  content: "\e184";
}
.glyphicon-stats:before {
  content: "\e185";
}
.glyphicon-sd-video:before {
  content: "\e186";
}
.glyphicon-hd-video:before {
  content: "\e187";
}
.glyphicon-subtitles:before {
  content: "\e188";
}
.glyphicon-sound-stereo:before {
  content: "\e189";
}
.glyphicon-sound-dolby:before {
  content: "\e190";
}
.glyphicon-sound-5-1:before {
  content: "\e191";
}
.glyphicon-sound-6-1:before {
  content: "\e192";
}
.glyphicon-sound-7-1:before {
  content: "\e193";
}
.glyphicon-copyright-mark:before {
  content: "\e194";
}
.glyphicon-registration-mark:before {
  content: "\e195";
}
.glyphicon-cloud-download:before {
  content: "\e197";
}
.glyphicon-cloud-upload:before {
  content: "\e198";
}
.glyphicon-tree-conifer:before {
  content: "\e199";
}
.glyphicon-tree-deciduous:before {
  content: "\e200";
}
.glyphicon-cd:before {
  content: "\e201";
}
.glyphicon-save-file:before {
  content: "\e202";
}
.glyphicon-open-file:before {
  content: "\e203";
}
.glyphicon-level-up:before {
  content: "\e204";
}
.glyphicon-copy:before {
  content: "\e205";
}
.glyphicon-paste:before {
  content: "\e206";
}
.glyphicon-alert:before {
  content: "\e209";
}
.glyphicon-equalizer:before {
  content: "\e210";
}
.glyphicon-king:before {
  content: "\e211";
}
.glyphicon-queen:before {
  content: "\e212";
}
.glyphicon-pawn:before {
  content: "\e213";
}
.glyphicon-bishop:before {
  content: "\e214";
}
.glyphicon-knight:before {
  content: "\e215";
}
.glyphicon-baby-formula:before {
  content: "\e216";
}
.glyphicon-tent:before {
  content: "\26fa";
}
.glyphicon-blackboard:before {
  content: "\e218";
}
.glyphicon-bed:before {
  content: "\e219";
}
.glyphicon-apple:before {
  content: "\f8ff";
}
.glyphicon-erase:before {
  content: "\e221";
}
.glyphicon-hourglass:before {
  content: "\231b";
}
.glyphicon-lamp:before {
  content: "\e223";
}
.glyphicon-duplicate:before {
  content: "\e224";
}
.glyphicon-piggy-bank:before {
  content: "\e225";
}
.glyphicon-scissors:before {
  content: "\e226";
}
.glyphicon-bitcoin:before {
  content: "\e227";
}
.glyphicon-btc:before {
  content: "\e227";
}
.glyphicon-xbt:before {
  content: "\e227";
}
.glyphicon-yen:before {
  content: "\00a5";
}
.glyphicon-jpy:before {
  content: "\00a5";
}
.glyphicon-ruble:before {
  content: "\20bd";
}
.glyphicon-rub:before {
  content: "\20bd";
}
.glyphicon-scale:before {
  content: "\e230";
}
.glyphicon-ice-lolly:before {
  content: "\e231";
}
.glyphicon-ice-lolly-tasted:before {
  content: "\e232";
}
.glyphicon-education:before {
  content: "\e233";
}
.glyphicon-option-horizontal:before {
  content: "\e234";
}
.glyphicon-option-vertical:before {
  content: "\e235";
}
.glyphicon-menu-hamburger:before {
  content: "\e236";
}
.glyphicon-modal-window:before {
  content: "\e237";
}
.glyphicon-oil:before {
  content: "\e238";
}
.glyphicon-grain:before {
  content: "\e239";
}
.glyphicon-sunglasses:before {
  content: "\e240";
}
.glyphicon-text-size:before {
  content: "\e241";
}
.glyphicon-text-color:before {
  content: "\e242";
}
.glyphicon-text-background:before {
  content: "\e243";
}
.glyphicon-object-align-top:before {
  content: "\e244";
}
.glyphicon-object-align-bottom:before {
  content: "\e245";
}
.glyphicon-object-align-horizontal:before {
  content: "\e246";
}
.glyphicon-object-align-left:before {
  content: "\e247";
}
.glyphicon-object-align-vertical:before {
  content: "\e248";
}
.glyphicon-object-align-right:before {
  content: "\e249";
}
.glyphicon-triangle-right:before {
  content: "\e250";
}
.glyphicon-triangle-left:before {
  content: "\e251";
}
.glyphicon-triangle-bottom:before {
  content: "\e252";
}
.glyphicon-triangle-top:before {
  content: "\e253";
}
.glyphicon-console:before {
  content: "\e254";
}
.glyphicon-superscript:before {
  content: "\e255";
}
.glyphicon-subscript:before {
  content: "\e256";
}
.glyphicon-menu-left:before {
  content: "\e257";
}
.glyphicon-menu-right:before {
  content: "\e258";
}
.glyphicon-menu-down:before {
  content: "\e259";
}
.glyphicon-menu-up:before {
  content: "\e260";
}
* {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
*:before,
*:after {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
html {
  font-size: 10px;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
body {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 13px;
  line-height: 1.42857143;
  color: #000;
  background-color: #fff;
}
input,
button,
select,
textarea {
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
}
a {
  color: #337ab7;
  text-decoration: none;
}
a:hover,
a:focus {
  color: #23527c;
  text-decoration: underline;
}
a:focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
figure {
  margin: 0;
}
img {
  vertical-align: middle;
}
.img-responsive,
.thumbnail > img,
.thumbnail a > img,
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  display: block;
  max-width: 100%;
  height: auto;
}
.img-rounded {
  border-radius: 3px;
}
.img-thumbnail {
  padding: 4px;
  line-height: 1.42857143;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 2px;
  -webkit-transition: all 0.2s ease-in-out;
  -o-transition: all 0.2s ease-in-out;
  transition: all 0.2s ease-in-out;
  display: inline-block;
  max-width: 100%;
  height: auto;
}
.img-circle {
  border-radius: 50%;
}
hr {
  margin-top: 18px;
  margin-bottom: 18px;
  border: 0;
  border-top: 1px solid #eeeeee;
}
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
[role="button"] {
  cursor: pointer;
}
h1,
h2,
h3,
h4,
h5,
h6,
.h1,
.h2,
.h3,
.h4,
.h5,
.h6 {
  font-family: inherit;
  font-weight: 500;
  line-height: 1.1;
  color: inherit;
}
h1 small,
h2 small,
h3 small,
h4 small,
h5 small,
h6 small,
.h1 small,
.h2 small,
.h3 small,
.h4 small,
.h5 small,
.h6 small,
h1 .small,
h2 .small,
h3 .small,
h4 .small,
h5 .small,
h6 .small,
.h1 .small,
.h2 .small,
.h3 .small,
.h4 .small,
.h5 .small,
.h6 .small {
  font-weight: normal;
  line-height: 1;
  color: #777777;
}
h1,
.h1,
h2,
.h2,
h3,
.h3 {
  margin-top: 18px;
  margin-bottom: 9px;
}
h1 small,
.h1 small,
h2 small,
.h2 small,
h3 small,
.h3 small,
h1 .small,
.h1 .small,
h2 .small,
.h2 .small,
h3 .small,
.h3 .small {
  font-size: 65%;
}
h4,
.h4,
h5,
.h5,
h6,
.h6 {
  margin-top: 9px;
  margin-bottom: 9px;
}
h4 small,
.h4 small,
h5 small,
.h5 small,
h6 small,
.h6 small,
h4 .small,
.h4 .small,
h5 .small,
.h5 .small,
h6 .small,
.h6 .small {
  font-size: 75%;
}
h1,
.h1 {
  font-size: 33px;
}
h2,
.h2 {
  font-size: 27px;
}
h3,
.h3 {
  font-size: 23px;
}
h4,
.h4 {
  font-size: 17px;
}
h5,
.h5 {
  font-size: 13px;
}
h6,
.h6 {
  font-size: 12px;
}
p {
  margin: 0 0 9px;
}
.lead {
  margin-bottom: 18px;
  font-size: 14px;
  font-weight: 300;
  line-height: 1.4;
}
@media (min-width: 768px) {
  .lead {
    font-size: 19.5px;
  }
}
small,
.small {
  font-size: 92%;
}
mark,
.mark {
  background-color: #fcf8e3;
  padding: .2em;
}
.text-left {
  text-align: left;
}
.text-right {
  text-align: right;
}
.text-center {
  text-align: center;
}
.text-justify {
  text-align: justify;
}
.text-nowrap {
  white-space: nowrap;
}
.text-lowercase {
  text-transform: lowercase;
}
.text-uppercase {
  text-transform: uppercase;
}
.text-capitalize {
  text-transform: capitalize;
}
.text-muted {
  color: #777777;
}
.text-primary {
  color: #337ab7;
}
a.text-primary:hover,
a.text-primary:focus {
  color: #286090;
}
.text-success {
  color: #3c763d;
}
a.text-success:hover,
a.text-success:focus {
  color: #2b542c;
}
.text-info {
  color: #31708f;
}
a.text-info:hover,
a.text-info:focus {
  color: #245269;
}
.text-warning {
  color: #8a6d3b;
}
a.text-warning:hover,
a.text-warning:focus {
  color: #66512c;
}
.text-danger {
  color: #a94442;
}
a.text-danger:hover,
a.text-danger:focus {
  color: #843534;
}
.bg-primary {
  color: #fff;
  background-color: #337ab7;
}
a.bg-primary:hover,
a.bg-primary:focus {
  background-color: #286090;
}
.bg-success {
  background-color: #dff0d8;
}
a.bg-success:hover,
a.bg-success:focus {
  background-color: #c1e2b3;
}
.bg-info {
  background-color: #d9edf7;
}
a.bg-info:hover,
a.bg-info:focus {
  background-color: #afd9ee;
}
.bg-warning {
  background-color: #fcf8e3;
}
a.bg-warning:hover,
a.bg-warning:focus {
  background-color: #f7ecb5;
}
.bg-danger {
  background-color: #f2dede;
}
a.bg-danger:hover,
a.bg-danger:focus {
  background-color: #e4b9b9;
}
.page-header {
  padding-bottom: 8px;
  margin: 36px 0 18px;
  border-bottom: 1px solid #eeeeee;
}
ul,
ol {
  margin-top: 0;
  margin-bottom: 9px;
}
ul ul,
ol ul,
ul ol,
ol ol {
  margin-bottom: 0;
}
.list-unstyled {
  padding-left: 0;
  list-style: none;
}
.list-inline {
  padding-left: 0;
  list-style: none;
  margin-left: -5px;
}
.list-inline > li {
  display: inline-block;
  padding-left: 5px;
  padding-right: 5px;
}
dl {
  margin-top: 0;
  margin-bottom: 18px;
}
dt,
dd {
  line-height: 1.42857143;
}
dt {
  font-weight: bold;
}
dd {
  margin-left: 0;
}
@media (min-width: 541px) {
  .dl-horizontal dt {
    float: left;
    width: 160px;
    clear: left;
    text-align: right;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .dl-horizontal dd {
    margin-left: 180px;
  }
}
abbr[title],
abbr[data-original-title] {
  cursor: help;
  border-bottom: 1px dotted #777777;
}
.initialism {
  font-size: 90%;
  text-transform: uppercase;
}
blockquote {
  padding: 9px 18px;
  margin: 0 0 18px;
  font-size: inherit;
  border-left: 5px solid #eeeeee;
}
blockquote p:last-child,
blockquote ul:last-child,
blockquote ol:last-child {
  margin-bottom: 0;
}
blockquote footer,
blockquote small,
blockquote .small {
  display: block;
  font-size: 80%;
  line-height: 1.42857143;
  color: #777777;
}
blockquote footer:before,
blockquote small:before,
blockquote .small:before {
  content: '\2014 \00A0';
}
.blockquote-reverse,
blockquote.pull-right {
  padding-right: 15px;
  padding-left: 0;
  border-right: 5px solid #eeeeee;
  border-left: 0;
  text-align: right;
}
.blockquote-reverse footer:before,
blockquote.pull-right footer:before,
.blockquote-reverse small:before,
blockquote.pull-right small:before,
.blockquote-reverse .small:before,
blockquote.pull-right .small:before {
  content: '';
}
.blockquote-reverse footer:after,
blockquote.pull-right footer:after,
.blockquote-reverse small:after,
blockquote.pull-right small:after,
.blockquote-reverse .small:after,
blockquote.pull-right .small:after {
  content: '\00A0 \2014';
}
address {
  margin-bottom: 18px;
  font-style: normal;
  line-height: 1.42857143;
}
code,
kbd,
pre,
samp {
  font-family: monospace;
}
code {
  padding: 2px 4px;
  font-size: 90%;
  color: #c7254e;
  background-color: #f9f2f4;
  border-radius: 2px;
}
kbd {
  padding: 2px 4px;
  font-size: 90%;
  color: #888;
  background-color: transparent;
  border-radius: 1px;
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.25);
}
kbd kbd {
  padding: 0;
  font-size: 100%;
  font-weight: bold;
  box-shadow: none;
}
pre {
  display: block;
  padding: 8.5px;
  margin: 0 0 9px;
  font-size: 12px;
  line-height: 1.42857143;
  word-break: break-all;
  word-wrap: break-word;
  color: #333333;
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 2px;
}
pre code {
  padding: 0;
  font-size: inherit;
  color: inherit;
  white-space: pre-wrap;
  background-color: transparent;
  border-radius: 0;
}
.pre-scrollable {
  max-height: 340px;
  overflow-y: scroll;
}
.container {
  margin-right: auto;
  margin-left: auto;
  padding-left: 0px;
  padding-right: 0px;
}
@media (min-width: 768px) {
  .container {
    width: 768px;
  }
}
@media (min-width: 992px) {
  .container {
    width: 940px;
  }
}
@media (min-width: 1200px) {
  .container {
    width: 1140px;
  }
}
.container-fluid {
  margin-right: auto;
  margin-left: auto;
  padding-left: 0px;
  padding-right: 0px;
}
.row {
  margin-left: 0px;
  margin-right: 0px;
}
.col-xs-1, .col-sm-1, .col-md-1, .col-lg-1, .col-xs-2, .col-sm-2, .col-md-2, .col-lg-2, .col-xs-3, .col-sm-3, .col-md-3, .col-lg-3, .col-xs-4, .col-sm-4, .col-md-4, .col-lg-4, .col-xs-5, .col-sm-5, .col-md-5, .col-lg-5, .col-xs-6, .col-sm-6, .col-md-6, .col-lg-6, .col-xs-7, .col-sm-7, .col-md-7, .col-lg-7, .col-xs-8, .col-sm-8, .col-md-8, .col-lg-8, .col-xs-9, .col-sm-9, .col-md-9, .col-lg-9, .col-xs-10, .col-sm-10, .col-md-10, .col-lg-10, .col-xs-11, .col-sm-11, .col-md-11, .col-lg-11, .col-xs-12, .col-sm-12, .col-md-12, .col-lg-12 {
  position: relative;
  min-height: 1px;
  padding-left: 0px;
  padding-right: 0px;
}
.col-xs-1, .col-xs-2, .col-xs-3, .col-xs-4, .col-xs-5, .col-xs-6, .col-xs-7, .col-xs-8, .col-xs-9, .col-xs-10, .col-xs-11, .col-xs-12 {
  float: left;
}
.col-xs-12 {
  width: 100%;
}
.col-xs-11 {
  width: 91.66666667%;
}
.col-xs-10 {
  width: 83.33333333%;
}
.col-xs-9 {
  width: 75%;
}
.col-xs-8 {
  width: 66.66666667%;
}
.col-xs-7 {
  width: 58.33333333%;
}
.col-xs-6 {
  width: 50%;
}
.col-xs-5 {
  width: 41.66666667%;
}
.col-xs-4 {
  width: 33.33333333%;
}
.col-xs-3 {
  width: 25%;
}
.col-xs-2 {
  width: 16.66666667%;
}
.col-xs-1 {
  width: 8.33333333%;
}
.col-xs-pull-12 {
  right: 100%;
}
.col-xs-pull-11 {
  right: 91.66666667%;
}
.col-xs-pull-10 {
  right: 83.33333333%;
}
.col-xs-pull-9 {
  right: 75%;
}
.col-xs-pull-8 {
  right: 66.66666667%;
}
.col-xs-pull-7 {
  right: 58.33333333%;
}
.col-xs-pull-6 {
  right: 50%;
}
.col-xs-pull-5 {
  right: 41.66666667%;
}
.col-xs-pull-4 {
  right: 33.33333333%;
}
.col-xs-pull-3 {
  right: 25%;
}
.col-xs-pull-2 {
  right: 16.66666667%;
}
.col-xs-pull-1 {
  right: 8.33333333%;
}
.col-xs-pull-0 {
  right: auto;
}
.col-xs-push-12 {
  left: 100%;
}
.col-xs-push-11 {
  left: 91.66666667%;
}
.col-xs-push-10 {
  left: 83.33333333%;
}
.col-xs-push-9 {
  left: 75%;
}
.col-xs-push-8 {
  left: 66.66666667%;
}
.col-xs-push-7 {
  left: 58.33333333%;
}
.col-xs-push-6 {
  left: 50%;
}
.col-xs-push-5 {
  left: 41.66666667%;
}
.col-xs-push-4 {
  left: 33.33333333%;
}
.col-xs-push-3 {
  left: 25%;
}
.col-xs-push-2 {
  left: 16.66666667%;
}
.col-xs-push-1 {
  left: 8.33333333%;
}
.col-xs-push-0 {
  left: auto;
}
.col-xs-offset-12 {
  margin-left: 100%;
}
.col-xs-offset-11 {
  margin-left: 91.66666667%;
}
.col-xs-offset-10 {
  margin-left: 83.33333333%;
}
.col-xs-offset-9 {
  margin-left: 75%;
}
.col-xs-offset-8 {
  margin-left: 66.66666667%;
}
.col-xs-offset-7 {
  margin-left: 58.33333333%;
}
.col-xs-offset-6 {
  margin-left: 50%;
}
.col-xs-offset-5 {
  margin-left: 41.66666667%;
}
.col-xs-offset-4 {
  margin-left: 33.33333333%;
}
.col-xs-offset-3 {
  margin-left: 25%;
}
.col-xs-offset-2 {
  margin-left: 16.66666667%;
}
.col-xs-offset-1 {
  margin-left: 8.33333333%;
}
.col-xs-offset-0 {
  margin-left: 0%;
}
@media (min-width: 768px) {
  .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6, .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
    float: left;
  }
  .col-sm-12 {
    width: 100%;
  }
  .col-sm-11 {
    width: 91.66666667%;
  }
  .col-sm-10 {
    width: 83.33333333%;
  }
  .col-sm-9 {
    width: 75%;
  }
  .col-sm-8 {
    width: 66.66666667%;
  }
  .col-sm-7 {
    width: 58.33333333%;
  }
  .col-sm-6 {
    width: 50%;
  }
  .col-sm-5 {
    width: 41.66666667%;
  }
  .col-sm-4 {
    width: 33.33333333%;
  }
  .col-sm-3 {
    width: 25%;
  }
  .col-sm-2 {
    width: 16.66666667%;
  }
  .col-sm-1 {
    width: 8.33333333%;
  }
  .col-sm-pull-12 {
    right: 100%;
  }
  .col-sm-pull-11 {
    right: 91.66666667%;
  }
  .col-sm-pull-10 {
    right: 83.33333333%;
  }
  .col-sm-pull-9 {
    right: 75%;
  }
  .col-sm-pull-8 {
    right: 66.66666667%;
  }
  .col-sm-pull-7 {
    right: 58.33333333%;
  }
  .col-sm-pull-6 {
    right: 50%;
  }
  .col-sm-pull-5 {
    right: 41.66666667%;
  }
  .col-sm-pull-4 {
    right: 33.33333333%;
  }
  .col-sm-pull-3 {
    right: 25%;
  }
  .col-sm-pull-2 {
    right: 16.66666667%;
  }
  .col-sm-pull-1 {
    right: 8.33333333%;
  }
  .col-sm-pull-0 {
    right: auto;
  }
  .col-sm-push-12 {
    left: 100%;
  }
  .col-sm-push-11 {
    left: 91.66666667%;
  }
  .col-sm-push-10 {
    left: 83.33333333%;
  }
  .col-sm-push-9 {
    left: 75%;
  }
  .col-sm-push-8 {
    left: 66.66666667%;
  }
  .col-sm-push-7 {
    left: 58.33333333%;
  }
  .col-sm-push-6 {
    left: 50%;
  }
  .col-sm-push-5 {
    left: 41.66666667%;
  }
  .col-sm-push-4 {
    left: 33.33333333%;
  }
  .col-sm-push-3 {
    left: 25%;
  }
  .col-sm-push-2 {
    left: 16.66666667%;
  }
  .col-sm-push-1 {
    left: 8.33333333%;
  }
  .col-sm-push-0 {
    left: auto;
  }
  .col-sm-offset-12 {
    margin-left: 100%;
  }
  .col-sm-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-sm-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-sm-offset-9 {
    margin-left: 75%;
  }
  .col-sm-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-sm-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-sm-offset-6 {
    margin-left: 50%;
  }
  .col-sm-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-sm-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-sm-offset-3 {
    margin-left: 25%;
  }
  .col-sm-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-sm-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-sm-offset-0 {
    margin-left: 0%;
  }
}
@media (min-width: 992px) {
  .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6, .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12 {
    float: left;
  }
  .col-md-12 {
    width: 100%;
  }
  .col-md-11 {
    width: 91.66666667%;
  }
  .col-md-10 {
    width: 83.33333333%;
  }
  .col-md-9 {
    width: 75%;
  }
  .col-md-8 {
    width: 66.66666667%;
  }
  .col-md-7 {
    width: 58.33333333%;
  }
  .col-md-6 {
    width: 50%;
  }
  .col-md-5 {
    width: 41.66666667%;
  }
  .col-md-4 {
    width: 33.33333333%;
  }
  .col-md-3 {
    width: 25%;
  }
  .col-md-2 {
    width: 16.66666667%;
  }
  .col-md-1 {
    width: 8.33333333%;
  }
  .col-md-pull-12 {
    right: 100%;
  }
  .col-md-pull-11 {
    right: 91.66666667%;
  }
  .col-md-pull-10 {
    right: 83.33333333%;
  }
  .col-md-pull-9 {
    right: 75%;
  }
  .col-md-pull-8 {
    right: 66.66666667%;
  }
  .col-md-pull-7 {
    right: 58.33333333%;
  }
  .col-md-pull-6 {
    right: 50%;
  }
  .col-md-pull-5 {
    right: 41.66666667%;
  }
  .col-md-pull-4 {
    right: 33.33333333%;
  }
  .col-md-pull-3 {
    right: 25%;
  }
  .col-md-pull-2 {
    right: 16.66666667%;
  }
  .col-md-pull-1 {
    right: 8.33333333%;
  }
  .col-md-pull-0 {
    right: auto;
  }
  .col-md-push-12 {
    left: 100%;
  }
  .col-md-push-11 {
    left: 91.66666667%;
  }
  .col-md-push-10 {
    left: 83.33333333%;
  }
  .col-md-push-9 {
    left: 75%;
  }
  .col-md-push-8 {
    left: 66.66666667%;
  }
  .col-md-push-7 {
    left: 58.33333333%;
  }
  .col-md-push-6 {
    left: 50%;
  }
  .col-md-push-5 {
    left: 41.66666667%;
  }
  .col-md-push-4 {
    left: 33.33333333%;
  }
  .col-md-push-3 {
    left: 25%;
  }
  .col-md-push-2 {
    left: 16.66666667%;
  }
  .col-md-push-1 {
    left: 8.33333333%;
  }
  .col-md-push-0 {
    left: auto;
  }
  .col-md-offset-12 {
    margin-left: 100%;
  }
  .col-md-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-md-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-md-offset-9 {
    margin-left: 75%;
  }
  .col-md-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-md-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-md-offset-6 {
    margin-left: 50%;
  }
  .col-md-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-md-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-md-offset-3 {
    margin-left: 25%;
  }
  .col-md-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-md-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-md-offset-0 {
    margin-left: 0%;
  }
}
@media (min-width: 1200px) {
  .col-lg-1, .col-lg-2, .col-lg-3, .col-lg-4, .col-lg-5, .col-lg-6, .col-lg-7, .col-lg-8, .col-lg-9, .col-lg-10, .col-lg-11, .col-lg-12 {
    float: left;
  }
  .col-lg-12 {
    width: 100%;
  }
  .col-lg-11 {
    width: 91.66666667%;
  }
  .col-lg-10 {
    width: 83.33333333%;
  }
  .col-lg-9 {
    width: 75%;
  }
  .col-lg-8 {
    width: 66.66666667%;
  }
  .col-lg-7 {
    width: 58.33333333%;
  }
  .col-lg-6 {
    width: 50%;
  }
  .col-lg-5 {
    width: 41.66666667%;
  }
  .col-lg-4 {
    width: 33.33333333%;
  }
  .col-lg-3 {
    width: 25%;
  }
  .col-lg-2 {
    width: 16.66666667%;
  }
  .col-lg-1 {
    width: 8.33333333%;
  }
  .col-lg-pull-12 {
    right: 100%;
  }
  .col-lg-pull-11 {
    right: 91.66666667%;
  }
  .col-lg-pull-10 {
    right: 83.33333333%;
  }
  .col-lg-pull-9 {
    right: 75%;
  }
  .col-lg-pull-8 {
    right: 66.66666667%;
  }
  .col-lg-pull-7 {
    right: 58.33333333%;
  }
  .col-lg-pull-6 {
    right: 50%;
  }
  .col-lg-pull-5 {
    right: 41.66666667%;
  }
  .col-lg-pull-4 {
    right: 33.33333333%;
  }
  .col-lg-pull-3 {
    right: 25%;
  }
  .col-lg-pull-2 {
    right: 16.66666667%;
  }
  .col-lg-pull-1 {
    right: 8.33333333%;
  }
  .col-lg-pull-0 {
    right: auto;
  }
  .col-lg-push-12 {
    left: 100%;
  }
  .col-lg-push-11 {
    left: 91.66666667%;
  }
  .col-lg-push-10 {
    left: 83.33333333%;
  }
  .col-lg-push-9 {
    left: 75%;
  }
  .col-lg-push-8 {
    left: 66.66666667%;
  }
  .col-lg-push-7 {
    left: 58.33333333%;
  }
  .col-lg-push-6 {
    left: 50%;
  }
  .col-lg-push-5 {
    left: 41.66666667%;
  }
  .col-lg-push-4 {
    left: 33.33333333%;
  }
  .col-lg-push-3 {
    left: 25%;
  }
  .col-lg-push-2 {
    left: 16.66666667%;
  }
  .col-lg-push-1 {
    left: 8.33333333%;
  }
  .col-lg-push-0 {
    left: auto;
  }
  .col-lg-offset-12 {
    margin-left: 100%;
  }
  .col-lg-offset-11 {
    margin-left: 91.66666667%;
  }
  .col-lg-offset-10 {
    margin-left: 83.33333333%;
  }
  .col-lg-offset-9 {
    margin-left: 75%;
  }
  .col-lg-offset-8 {
    margin-left: 66.66666667%;
  }
  .col-lg-offset-7 {
    margin-left: 58.33333333%;
  }
  .col-lg-offset-6 {
    margin-left: 50%;
  }
  .col-lg-offset-5 {
    margin-left: 41.66666667%;
  }
  .col-lg-offset-4 {
    margin-left: 33.33333333%;
  }
  .col-lg-offset-3 {
    margin-left: 25%;
  }
  .col-lg-offset-2 {
    margin-left: 16.66666667%;
  }
  .col-lg-offset-1 {
    margin-left: 8.33333333%;
  }
  .col-lg-offset-0 {
    margin-left: 0%;
  }
}
table {
  background-color: transparent;
}
caption {
  padding-top: 8px;
  padding-bottom: 8px;
  color: #777777;
  text-align: left;
}
th {
  text-align: left;
}
.table {
  width: 100%;
  max-width: 100%;
  margin-bottom: 18px;
}
.table > thead > tr > th,
.table > tbody > tr > th,
.table > tfoot > tr > th,
.table > thead > tr > td,
.table > tbody > tr > td,
.table > tfoot > tr > td {
  padding: 8px;
  line-height: 1.42857143;
  vertical-align: top;
  border-top: 1px solid #ddd;
}
.table > thead > tr > th {
  vertical-align: bottom;
  border-bottom: 2px solid #ddd;
}
.table > caption + thead > tr:first-child > th,
.table > colgroup + thead > tr:first-child > th,
.table > thead:first-child > tr:first-child > th,
.table > caption + thead > tr:first-child > td,
.table > colgroup + thead > tr:first-child > td,
.table > thead:first-child > tr:first-child > td {
  border-top: 0;
}
.table > tbody + tbody {
  border-top: 2px solid #ddd;
}
.table .table {
  background-color: #fff;
}
.table-condensed > thead > tr > th,
.table-condensed > tbody > tr > th,
.table-condensed > tfoot > tr > th,
.table-condensed > thead > tr > td,
.table-condensed > tbody > tr > td,
.table-condensed > tfoot > tr > td {
  padding: 5px;
}
.table-bordered {
  border: 1px solid #ddd;
}
.table-bordered > thead > tr > th,
.table-bordered > tbody > tr > th,
.table-bordered > tfoot > tr > th,
.table-bordered > thead > tr > td,
.table-bordered > tbody > tr > td,
.table-bordered > tfoot > tr > td {
  border: 1px solid #ddd;
}
.table-bordered > thead > tr > th,
.table-bordered > thead > tr > td {
  border-bottom-width: 2px;
}
.table-striped > tbody > tr:nth-of-type(odd) {
  background-color: #f9f9f9;
}
.table-hover > tbody > tr:hover {
  background-color: #f5f5f5;
}
table col[class*="col-"] {
  position: static;
  float: none;
  display: table-column;
}
table td[class*="col-"],
table th[class*="col-"] {
  position: static;
  float: none;
  display: table-cell;
}
.table > thead > tr > td.active,
.table > tbody > tr > td.active,
.table > tfoot > tr > td.active,
.table > thead > tr > th.active,
.table > tbody > tr > th.active,
.table > tfoot > tr > th.active,
.table > thead > tr.active > td,
.table > tbody > tr.active > td,
.table > tfoot > tr.active > td,
.table > thead > tr.active > th,
.table > tbody > tr.active > th,
.table > tfoot > tr.active > th {
  background-color: #f5f5f5;
}
.table-hover > tbody > tr > td.active:hover,
.table-hover > tbody > tr > th.active:hover,
.table-hover > tbody > tr.active:hover > td,
.table-hover > tbody > tr:hover > .active,
.table-hover > tbody > tr.active:hover > th {
  background-color: #e8e8e8;
}
.table > thead > tr > td.success,
.table > tbody > tr > td.success,
.table > tfoot > tr > td.success,
.table > thead > tr > th.success,
.table > tbody > tr > th.success,
.table > tfoot > tr > th.success,
.table > thead > tr.success > td,
.table > tbody > tr.success > td,
.table > tfoot > tr.success > td,
.table > thead > tr.success > th,
.table > tbody > tr.success > th,
.table > tfoot > tr.success > th {
  background-color: #dff0d8;
}
.table-hover > tbody > tr > td.success:hover,
.table-hover > tbody > tr > th.success:hover,
.table-hover > tbody > tr.success:hover > td,
.table-hover > tbody > tr:hover > .success,
.table-hover > tbody > tr.success:hover > th {
  background-color: #d0e9c6;
}
.table > thead > tr > td.info,
.table > tbody > tr > td.info,
.table > tfoot > tr > td.info,
.table > thead > tr > th.info,
.table > tbody > tr > th.info,
.table > tfoot > tr > th.info,
.table > thead > tr.info > td,
.table > tbody > tr.info > td,
.table > tfoot > tr.info > td,
.table > thead > tr.info > th,
.table > tbody > tr.info > th,
.table > tfoot > tr.info > th {
  background-color: #d9edf7;
}
.table-hover > tbody > tr > td.info:hover,
.table-hover > tbody > tr > th.info:hover,
.table-hover > tbody > tr.info:hover > td,
.table-hover > tbody > tr:hover > .info,
.table-hover > tbody > tr.info:hover > th {
  background-color: #c4e3f3;
}
.table > thead > tr > td.warning,
.table > tbody > tr > td.warning,
.table > tfoot > tr > td.warning,
.table > thead > tr > th.warning,
.table > tbody > tr > th.warning,
.table > tfoot > tr > th.warning,
.table > thead > tr.warning > td,
.table > tbody > tr.warning > td,
.table > tfoot > tr.warning > td,
.table > thead > tr.warning > th,
.table > tbody > tr.warning > th,
.table > tfoot > tr.warning > th {
  background-color: #fcf8e3;
}
.table-hover > tbody > tr > td.warning:hover,
.table-hover > tbody > tr > th.warning:hover,
.table-hover > tbody > tr.warning:hover > td,
.table-hover > tbody > tr:hover > .warning,
.table-hover > tbody > tr.warning:hover > th {
  background-color: #faf2cc;
}
.table > thead > tr > td.danger,
.table > tbody > tr > td.danger,
.table > tfoot > tr > td.danger,
.table > thead > tr > th.danger,
.table > tbody > tr > th.danger,
.table > tfoot > tr > th.danger,
.table > thead > tr.danger > td,
.table > tbody > tr.danger > td,
.table > tfoot > tr.danger > td,
.table > thead > tr.danger > th,
.table > tbody > tr.danger > th,
.table > tfoot > tr.danger > th {
  background-color: #f2dede;
}
.table-hover > tbody > tr > td.danger:hover,
.table-hover > tbody > tr > th.danger:hover,
.table-hover > tbody > tr.danger:hover > td,
.table-hover > tbody > tr:hover > .danger,
.table-hover > tbody > tr.danger:hover > th {
  background-color: #ebcccc;
}
.table-responsive {
  overflow-x: auto;
  min-height: 0.01%;
}
@media screen and (max-width: 767px) {
  .table-responsive {
    width: 100%;
    margin-bottom: 13.5px;
    overflow-y: hidden;
    -ms-overflow-style: -ms-autohiding-scrollbar;
    border: 1px solid #ddd;
  }
  .table-responsive > .table {
    margin-bottom: 0;
  }
  .table-responsive > .table > thead > tr > th,
  .table-responsive > .table > tbody > tr > th,
  .table-responsive > .table > tfoot > tr > th,
  .table-responsive > .table > thead > tr > td,
  .table-responsive > .table > tbody > tr > td,
  .table-responsive > .table > tfoot > tr > td {
    white-space: nowrap;
  }
  .table-responsive > .table-bordered {
    border: 0;
  }
  .table-responsive > .table-bordered > thead > tr > th:first-child,
  .table-responsive > .table-bordered > tbody > tr > th:first-child,
  .table-responsive > .table-bordered > tfoot > tr > th:first-child,
  .table-responsive > .table-bordered > thead > tr > td:first-child,
  .table-responsive > .table-bordered > tbody > tr > td:first-child,
  .table-responsive > .table-bordered > tfoot > tr > td:first-child {
    border-left: 0;
  }
  .table-responsive > .table-bordered > thead > tr > th:last-child,
  .table-responsive > .table-bordered > tbody > tr > th:last-child,
  .table-responsive > .table-bordered > tfoot > tr > th:last-child,
  .table-responsive > .table-bordered > thead > tr > td:last-child,
  .table-responsive > .table-bordered > tbody > tr > td:last-child,
  .table-responsive > .table-bordered > tfoot > tr > td:last-child {
    border-right: 0;
  }
  .table-responsive > .table-bordered > tbody > tr:last-child > th,
  .table-responsive > .table-bordered > tfoot > tr:last-child > th,
  .table-responsive > .table-bordered > tbody > tr:last-child > td,
  .table-responsive > .table-bordered > tfoot > tr:last-child > td {
    border-bottom: 0;
  }
}
fieldset {
  padding: 0;
  margin: 0;
  border: 0;
  min-width: 0;
}
legend {
  display: block;
  width: 100%;
  padding: 0;
  margin-bottom: 18px;
  font-size: 19.5px;
  line-height: inherit;
  color: #333333;
  border: 0;
  border-bottom: 1px solid #e5e5e5;
}
label {
  display: inline-block;
  max-width: 100%;
  margin-bottom: 5px;
  font-weight: bold;
}
input[type="search"] {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
input[type="radio"],
input[type="checkbox"] {
  margin: 4px 0 0;
  margin-top: 1px \9;
  line-height: normal;
}
input[type="file"] {
  display: block;
}
input[type="range"] {
  display: block;
  width: 100%;
}
select[multiple],
select[size] {
  height: auto;
}
input[type="file"]:focus,
input[type="radio"]:focus,
input[type="checkbox"]:focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
output {
  display: block;
  padding-top: 7px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
}
.form-control {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
}
.form-control:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.form-control::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.form-control:-ms-input-placeholder {
  color: #999;
}
.form-control::-webkit-input-placeholder {
  color: #999;
}
.form-control::-ms-expand {
  border: 0;
  background-color: transparent;
}
.form-control[disabled],
.form-control[readonly],
fieldset[disabled] .form-control {
  background-color: #eeeeee;
  opacity: 1;
}
.form-control[disabled],
fieldset[disabled] .form-control {
  cursor: not-allowed;
}
textarea.form-control {
  height: auto;
}
input[type="search"] {
  -webkit-appearance: none;
}
@media screen and (-webkit-min-device-pixel-ratio: 0) {
  input[type="date"].form-control,
  input[type="time"].form-control,
  input[type="datetime-local"].form-control,
  input[type="month"].form-control {
    line-height: 32px;
  }
  input[type="date"].input-sm,
  input[type="time"].input-sm,
  input[type="datetime-local"].input-sm,
  input[type="month"].input-sm,
  .input-group-sm input[type="date"],
  .input-group-sm input[type="time"],
  .input-group-sm input[type="datetime-local"],
  .input-group-sm input[type="month"] {
    line-height: 30px;
  }
  input[type="date"].input-lg,
  input[type="time"].input-lg,
  input[type="datetime-local"].input-lg,
  input[type="month"].input-lg,
  .input-group-lg input[type="date"],
  .input-group-lg input[type="time"],
  .input-group-lg input[type="datetime-local"],
  .input-group-lg input[type="month"] {
    line-height: 45px;
  }
}
.form-group {
  margin-bottom: 15px;
}
.radio,
.checkbox {
  position: relative;
  display: block;
  margin-top: 10px;
  margin-bottom: 10px;
}
.radio label,
.checkbox label {
  min-height: 18px;
  padding-left: 20px;
  margin-bottom: 0;
  font-weight: normal;
  cursor: pointer;
}
.radio input[type="radio"],
.radio-inline input[type="radio"],
.checkbox input[type="checkbox"],
.checkbox-inline input[type="checkbox"] {
  position: absolute;
  margin-left: -20px;
  margin-top: 4px \9;
}
.radio + .radio,
.checkbox + .checkbox {
  margin-top: -5px;
}
.radio-inline,
.checkbox-inline {
  position: relative;
  display: inline-block;
  padding-left: 20px;
  margin-bottom: 0;
  vertical-align: middle;
  font-weight: normal;
  cursor: pointer;
}
.radio-inline + .radio-inline,
.checkbox-inline + .checkbox-inline {
  margin-top: 0;
  margin-left: 10px;
}
input[type="radio"][disabled],
input[type="checkbox"][disabled],
input[type="radio"].disabled,
input[type="checkbox"].disabled,
fieldset[disabled] input[type="radio"],
fieldset[disabled] input[type="checkbox"] {
  cursor: not-allowed;
}
.radio-inline.disabled,
.checkbox-inline.disabled,
fieldset[disabled] .radio-inline,
fieldset[disabled] .checkbox-inline {
  cursor: not-allowed;
}
.radio.disabled label,
.checkbox.disabled label,
fieldset[disabled] .radio label,
fieldset[disabled] .checkbox label {
  cursor: not-allowed;
}
.form-control-static {
  padding-top: 7px;
  padding-bottom: 7px;
  margin-bottom: 0;
  min-height: 31px;
}
.form-control-static.input-lg,
.form-control-static.input-sm {
  padding-left: 0;
  padding-right: 0;
}
.input-sm {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
select.input-sm {
  height: 30px;
  line-height: 30px;
}
textarea.input-sm,
select[multiple].input-sm {
  height: auto;
}
.form-group-sm .form-control {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.form-group-sm select.form-control {
  height: 30px;
  line-height: 30px;
}
.form-group-sm textarea.form-control,
.form-group-sm select[multiple].form-control {
  height: auto;
}
.form-group-sm .form-control-static {
  height: 30px;
  min-height: 30px;
  padding: 6px 10px;
  font-size: 12px;
  line-height: 1.5;
}
.input-lg {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
select.input-lg {
  height: 45px;
  line-height: 45px;
}
textarea.input-lg,
select[multiple].input-lg {
  height: auto;
}
.form-group-lg .form-control {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
.form-group-lg select.form-control {
  height: 45px;
  line-height: 45px;
}
.form-group-lg textarea.form-control,
.form-group-lg select[multiple].form-control {
  height: auto;
}
.form-group-lg .form-control-static {
  height: 45px;
  min-height: 35px;
  padding: 11px 16px;
  font-size: 17px;
  line-height: 1.3333333;
}
.has-feedback {
  position: relative;
}
.has-feedback .form-control {
  padding-right: 40px;
}
.form-control-feedback {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 2;
  display: block;
  width: 32px;
  height: 32px;
  line-height: 32px;
  text-align: center;
  pointer-events: none;
}
.input-lg + .form-control-feedback,
.input-group-lg + .form-control-feedback,
.form-group-lg .form-control + .form-control-feedback {
  width: 45px;
  height: 45px;
  line-height: 45px;
}
.input-sm + .form-control-feedback,
.input-group-sm + .form-control-feedback,
.form-group-sm .form-control + .form-control-feedback {
  width: 30px;
  height: 30px;
  line-height: 30px;
}
.has-success .help-block,
.has-success .control-label,
.has-success .radio,
.has-success .checkbox,
.has-success .radio-inline,
.has-success .checkbox-inline,
.has-success.radio label,
.has-success.checkbox label,
.has-success.radio-inline label,
.has-success.checkbox-inline label {
  color: #3c763d;
}
.has-success .form-control {
  border-color: #3c763d;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-success .form-control:focus {
  border-color: #2b542c;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #67b168;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #67b168;
}
.has-success .input-group-addon {
  color: #3c763d;
  border-color: #3c763d;
  background-color: #dff0d8;
}
.has-success .form-control-feedback {
  color: #3c763d;
}
.has-warning .help-block,
.has-warning .control-label,
.has-warning .radio,
.has-warning .checkbox,
.has-warning .radio-inline,
.has-warning .checkbox-inline,
.has-warning.radio label,
.has-warning.checkbox label,
.has-warning.radio-inline label,
.has-warning.checkbox-inline label {
  color: #8a6d3b;
}
.has-warning .form-control {
  border-color: #8a6d3b;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-warning .form-control:focus {
  border-color: #66512c;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #c0a16b;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #c0a16b;
}
.has-warning .input-group-addon {
  color: #8a6d3b;
  border-color: #8a6d3b;
  background-color: #fcf8e3;
}
.has-warning .form-control-feedback {
  color: #8a6d3b;
}
.has-error .help-block,
.has-error .control-label,
.has-error .radio,
.has-error .checkbox,
.has-error .radio-inline,
.has-error .checkbox-inline,
.has-error.radio label,
.has-error.checkbox label,
.has-error.radio-inline label,
.has-error.checkbox-inline label {
  color: #a94442;
}
.has-error .form-control {
  border-color: #a94442;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
}
.has-error .form-control:focus {
  border-color: #843534;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #ce8483;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 6px #ce8483;
}
.has-error .input-group-addon {
  color: #a94442;
  border-color: #a94442;
  background-color: #f2dede;
}
.has-error .form-control-feedback {
  color: #a94442;
}
.has-feedback label ~ .form-control-feedback {
  top: 23px;
}
.has-feedback label.sr-only ~ .form-control-feedback {
  top: 0;
}
.help-block {
  display: block;
  margin-top: 5px;
  margin-bottom: 10px;
  color: #404040;
}
@media (min-width: 768px) {
  .form-inline .form-group {
    display: inline-block;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .form-control {
    display: inline-block;
    width: auto;
    vertical-align: middle;
  }
  .form-inline .form-control-static {
    display: inline-block;
  }
  .form-inline .input-group {
    display: inline-table;
    vertical-align: middle;
  }
  .form-inline .input-group .input-group-addon,
  .form-inline .input-group .input-group-btn,
  .form-inline .input-group .form-control {
    width: auto;
  }
  .form-inline .input-group > .form-control {
    width: 100%;
  }
  .form-inline .control-label {
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .radio,
  .form-inline .checkbox {
    display: inline-block;
    margin-top: 0;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .form-inline .radio label,
  .form-inline .checkbox label {
    padding-left: 0;
  }
  .form-inline .radio input[type="radio"],
  .form-inline .checkbox input[type="checkbox"] {
    position: relative;
    margin-left: 0;
  }
  .form-inline .has-feedback .form-control-feedback {
    top: 0;
  }
}
.form-horizontal .radio,
.form-horizontal .checkbox,
.form-horizontal .radio-inline,
.form-horizontal .checkbox-inline {
  margin-top: 0;
  margin-bottom: 0;
  padding-top: 7px;
}
.form-horizontal .radio,
.form-horizontal .checkbox {
  min-height: 25px;
}
.form-horizontal .form-group {
  margin-left: 0px;
  margin-right: 0px;
}
@media (min-width: 768px) {
  .form-horizontal .control-label {
    text-align: right;
    margin-bottom: 0;
    padding-top: 7px;
  }
}
.form-horizontal .has-feedback .form-control-feedback {
  right: 0px;
}
@media (min-width: 768px) {
  .form-horizontal .form-group-lg .control-label {
    padding-top: 11px;
    font-size: 17px;
  }
}
@media (min-width: 768px) {
  .form-horizontal .form-group-sm .control-label {
    padding-top: 6px;
    font-size: 12px;
  }
}
.btn {
  display: inline-block;
  margin-bottom: 0;
  font-weight: normal;
  text-align: center;
  vertical-align: middle;
  touch-action: manipulation;
  cursor: pointer;
  background-image: none;
  border: 1px solid transparent;
  white-space: nowrap;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  border-radius: 2px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
.btn:focus,
.btn:active:focus,
.btn.active:focus,
.btn.focus,
.btn:active.focus,
.btn.active.focus {
  outline: 5px auto -webkit-focus-ring-color;
  outline-offset: -2px;
}
.btn:hover,
.btn:focus,
.btn.focus {
  color: #333;
  text-decoration: none;
}
.btn:active,
.btn.active {
  outline: 0;
  background-image: none;
  -webkit-box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
}
.btn.disabled,
.btn[disabled],
fieldset[disabled] .btn {
  cursor: not-allowed;
  opacity: 0.65;
  filter: alpha(opacity=65);
  -webkit-box-shadow: none;
  box-shadow: none;
}
a.btn.disabled,
fieldset[disabled] a.btn {
  pointer-events: none;
}
.btn-default {
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
.btn-default:focus,
.btn-default.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
.btn-default:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.btn-default:active,
.btn-default.active,
.open > .dropdown-toggle.btn-default {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.btn-default:active:hover,
.btn-default.active:hover,
.open > .dropdown-toggle.btn-default:hover,
.btn-default:active:focus,
.btn-default.active:focus,
.open > .dropdown-toggle.btn-default:focus,
.btn-default:active.focus,
.btn-default.active.focus,
.open > .dropdown-toggle.btn-default.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
.btn-default:active,
.btn-default.active,
.open > .dropdown-toggle.btn-default {
  background-image: none;
}
.btn-default.disabled:hover,
.btn-default[disabled]:hover,
fieldset[disabled] .btn-default:hover,
.btn-default.disabled:focus,
.btn-default[disabled]:focus,
fieldset[disabled] .btn-default:focus,
.btn-default.disabled.focus,
.btn-default[disabled].focus,
fieldset[disabled] .btn-default.focus {
  background-color: #fff;
  border-color: #ccc;
}
.btn-default .badge {
  color: #fff;
  background-color: #333;
}
.btn-primary {
  color: #fff;
  background-color: #337ab7;
  border-color: #2e6da4;
}
.btn-primary:focus,
.btn-primary.focus {
  color: #fff;
  background-color: #286090;
  border-color: #122b40;
}
.btn-primary:hover {
  color: #fff;
  background-color: #286090;
  border-color: #204d74;
}
.btn-primary:active,
.btn-primary.active,
.open > .dropdown-toggle.btn-primary {
  color: #fff;
  background-color: #286090;
  border-color: #204d74;
}
.btn-primary:active:hover,
.btn-primary.active:hover,
.open > .dropdown-toggle.btn-primary:hover,
.btn-primary:active:focus,
.btn-primary.active:focus,
.open > .dropdown-toggle.btn-primary:focus,
.btn-primary:active.focus,
.btn-primary.active.focus,
.open > .dropdown-toggle.btn-primary.focus {
  color: #fff;
  background-color: #204d74;
  border-color: #122b40;
}
.btn-primary:active,
.btn-primary.active,
.open > .dropdown-toggle.btn-primary {
  background-image: none;
}
.btn-primary.disabled:hover,
.btn-primary[disabled]:hover,
fieldset[disabled] .btn-primary:hover,
.btn-primary.disabled:focus,
.btn-primary[disabled]:focus,
fieldset[disabled] .btn-primary:focus,
.btn-primary.disabled.focus,
.btn-primary[disabled].focus,
fieldset[disabled] .btn-primary.focus {
  background-color: #337ab7;
  border-color: #2e6da4;
}
.btn-primary .badge {
  color: #337ab7;
  background-color: #fff;
}
.btn-success {
  color: #fff;
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.btn-success:focus,
.btn-success.focus {
  color: #fff;
  background-color: #449d44;
  border-color: #255625;
}
.btn-success:hover {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.btn-success:active,
.btn-success.active,
.open > .dropdown-toggle.btn-success {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.btn-success:active:hover,
.btn-success.active:hover,
.open > .dropdown-toggle.btn-success:hover,
.btn-success:active:focus,
.btn-success.active:focus,
.open > .dropdown-toggle.btn-success:focus,
.btn-success:active.focus,
.btn-success.active.focus,
.open > .dropdown-toggle.btn-success.focus {
  color: #fff;
  background-color: #398439;
  border-color: #255625;
}
.btn-success:active,
.btn-success.active,
.open > .dropdown-toggle.btn-success {
  background-image: none;
}
.btn-success.disabled:hover,
.btn-success[disabled]:hover,
fieldset[disabled] .btn-success:hover,
.btn-success.disabled:focus,
.btn-success[disabled]:focus,
fieldset[disabled] .btn-success:focus,
.btn-success.disabled.focus,
.btn-success[disabled].focus,
fieldset[disabled] .btn-success.focus {
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.btn-success .badge {
  color: #5cb85c;
  background-color: #fff;
}
.btn-info {
  color: #fff;
  background-color: #5bc0de;
  border-color: #46b8da;
}
.btn-info:focus,
.btn-info.focus {
  color: #fff;
  background-color: #31b0d5;
  border-color: #1b6d85;
}
.btn-info:hover {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.btn-info:active,
.btn-info.active,
.open > .dropdown-toggle.btn-info {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.btn-info:active:hover,
.btn-info.active:hover,
.open > .dropdown-toggle.btn-info:hover,
.btn-info:active:focus,
.btn-info.active:focus,
.open > .dropdown-toggle.btn-info:focus,
.btn-info:active.focus,
.btn-info.active.focus,
.open > .dropdown-toggle.btn-info.focus {
  color: #fff;
  background-color: #269abc;
  border-color: #1b6d85;
}
.btn-info:active,
.btn-info.active,
.open > .dropdown-toggle.btn-info {
  background-image: none;
}
.btn-info.disabled:hover,
.btn-info[disabled]:hover,
fieldset[disabled] .btn-info:hover,
.btn-info.disabled:focus,
.btn-info[disabled]:focus,
fieldset[disabled] .btn-info:focus,
.btn-info.disabled.focus,
.btn-info[disabled].focus,
fieldset[disabled] .btn-info.focus {
  background-color: #5bc0de;
  border-color: #46b8da;
}
.btn-info .badge {
  color: #5bc0de;
  background-color: #fff;
}
.btn-warning {
  color: #fff;
  background-color: #f0ad4e;
  border-color: #eea236;
}
.btn-warning:focus,
.btn-warning.focus {
  color: #fff;
  background-color: #ec971f;
  border-color: #985f0d;
}
.btn-warning:hover {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.btn-warning:active,
.btn-warning.active,
.open > .dropdown-toggle.btn-warning {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.btn-warning:active:hover,
.btn-warning.active:hover,
.open > .dropdown-toggle.btn-warning:hover,
.btn-warning:active:focus,
.btn-warning.active:focus,
.open > .dropdown-toggle.btn-warning:focus,
.btn-warning:active.focus,
.btn-warning.active.focus,
.open > .dropdown-toggle.btn-warning.focus {
  color: #fff;
  background-color: #d58512;
  border-color: #985f0d;
}
.btn-warning:active,
.btn-warning.active,
.open > .dropdown-toggle.btn-warning {
  background-image: none;
}
.btn-warning.disabled:hover,
.btn-warning[disabled]:hover,
fieldset[disabled] .btn-warning:hover,
.btn-warning.disabled:focus,
.btn-warning[disabled]:focus,
fieldset[disabled] .btn-warning:focus,
.btn-warning.disabled.focus,
.btn-warning[disabled].focus,
fieldset[disabled] .btn-warning.focus {
  background-color: #f0ad4e;
  border-color: #eea236;
}
.btn-warning .badge {
  color: #f0ad4e;
  background-color: #fff;
}
.btn-danger {
  color: #fff;
  background-color: #d9534f;
  border-color: #d43f3a;
}
.btn-danger:focus,
.btn-danger.focus {
  color: #fff;
  background-color: #c9302c;
  border-color: #761c19;
}
.btn-danger:hover {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.btn-danger:active,
.btn-danger.active,
.open > .dropdown-toggle.btn-danger {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.btn-danger:active:hover,
.btn-danger.active:hover,
.open > .dropdown-toggle.btn-danger:hover,
.btn-danger:active:focus,
.btn-danger.active:focus,
.open > .dropdown-toggle.btn-danger:focus,
.btn-danger:active.focus,
.btn-danger.active.focus,
.open > .dropdown-toggle.btn-danger.focus {
  color: #fff;
  background-color: #ac2925;
  border-color: #761c19;
}
.btn-danger:active,
.btn-danger.active,
.open > .dropdown-toggle.btn-danger {
  background-image: none;
}
.btn-danger.disabled:hover,
.btn-danger[disabled]:hover,
fieldset[disabled] .btn-danger:hover,
.btn-danger.disabled:focus,
.btn-danger[disabled]:focus,
fieldset[disabled] .btn-danger:focus,
.btn-danger.disabled.focus,
.btn-danger[disabled].focus,
fieldset[disabled] .btn-danger.focus {
  background-color: #d9534f;
  border-color: #d43f3a;
}
.btn-danger .badge {
  color: #d9534f;
  background-color: #fff;
}
.btn-link {
  color: #337ab7;
  font-weight: normal;
  border-radius: 0;
}
.btn-link,
.btn-link:active,
.btn-link.active,
.btn-link[disabled],
fieldset[disabled] .btn-link {
  background-color: transparent;
  -webkit-box-shadow: none;
  box-shadow: none;
}
.btn-link,
.btn-link:hover,
.btn-link:focus,
.btn-link:active {
  border-color: transparent;
}
.btn-link:hover,
.btn-link:focus {
  color: #23527c;
  text-decoration: underline;
  background-color: transparent;
}
.btn-link[disabled]:hover,
fieldset[disabled] .btn-link:hover,
.btn-link[disabled]:focus,
fieldset[disabled] .btn-link:focus {
  color: #777777;
  text-decoration: none;
}
.btn-lg,
.btn-group-lg > .btn {
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
.btn-sm,
.btn-group-sm > .btn {
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.btn-xs,
.btn-group-xs > .btn {
  padding: 1px 5px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
.btn-block {
  display: block;
  width: 100%;
}
.btn-block + .btn-block {
  margin-top: 5px;
}
input[type="submit"].btn-block,
input[type="reset"].btn-block,
input[type="button"].btn-block {
  width: 100%;
}
.fade {
  opacity: 0;
  -webkit-transition: opacity 0.15s linear;
  -o-transition: opacity 0.15s linear;
  transition: opacity 0.15s linear;
}
.fade.in {
  opacity: 1;
}
.collapse {
  display: none;
}
.collapse.in {
  display: block;
}
tr.collapse.in {
  display: table-row;
}
tbody.collapse.in {
  display: table-row-group;
}
.collapsing {
  position: relative;
  height: 0;
  overflow: hidden;
  -webkit-transition-property: height, visibility;
  transition-property: height, visibility;
  -webkit-transition-duration: 0.35s;
  transition-duration: 0.35s;
  -webkit-transition-timing-function: ease;
  transition-timing-function: ease;
}
.caret {
  display: inline-block;
  width: 0;
  height: 0;
  margin-left: 2px;
  vertical-align: middle;
  border-top: 4px dashed;
  border-top: 4px solid \9;
  border-right: 4px solid transparent;
  border-left: 4px solid transparent;
}
.dropup,
.dropdown {
  position: relative;
}
.dropdown-toggle:focus {
  outline: 0;
}
.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1000;
  display: none;
  float: left;
  min-width: 160px;
  padding: 5px 0;
  margin: 2px 0 0;
  list-style: none;
  font-size: 13px;
  text-align: left;
  background-color: #fff;
  border: 1px solid #ccc;
  border: 1px solid rgba(0, 0, 0, 0.15);
  border-radius: 2px;
  -webkit-box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
  background-clip: padding-box;
}
.dropdown-menu.pull-right {
  right: 0;
  left: auto;
}
.dropdown-menu .divider {
  height: 1px;
  margin: 8px 0;
  overflow: hidden;
  background-color: #e5e5e5;
}
.dropdown-menu > li > a {
  display: block;
  padding: 3px 20px;
  clear: both;
  font-weight: normal;
  line-height: 1.42857143;
  color: #333333;
  white-space: nowrap;
}
.dropdown-menu > li > a:hover,
.dropdown-menu > li > a:focus {
  text-decoration: none;
  color: #262626;
  background-color: #f5f5f5;
}
.dropdown-menu > .active > a,
.dropdown-menu > .active > a:hover,
.dropdown-menu > .active > a:focus {
  color: #fff;
  text-decoration: none;
  outline: 0;
  background-color: #337ab7;
}
.dropdown-menu > .disabled > a,
.dropdown-menu > .disabled > a:hover,
.dropdown-menu > .disabled > a:focus {
  color: #777777;
}
.dropdown-menu > .disabled > a:hover,
.dropdown-menu > .disabled > a:focus {
  text-decoration: none;
  background-color: transparent;
  background-image: none;
  filter: progid:DXImageTransform.Microsoft.gradient(enabled = false);
  cursor: not-allowed;
}
.open > .dropdown-menu {
  display: block;
}
.open > a {
  outline: 0;
}
.dropdown-menu-right {
  left: auto;
  right: 0;
}
.dropdown-menu-left {
  left: 0;
  right: auto;
}
.dropdown-header {
  display: block;
  padding: 3px 20px;
  font-size: 12px;
  line-height: 1.42857143;
  color: #777777;
  white-space: nowrap;
}
.dropdown-backdrop {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  z-index: 990;
}
.pull-right > .dropdown-menu {
  right: 0;
  left: auto;
}
.dropup .caret,
.navbar-fixed-bottom .dropdown .caret {
  border-top: 0;
  border-bottom: 4px dashed;
  border-bottom: 4px solid \9;
  content: "";
}
.dropup .dropdown-menu,
.navbar-fixed-bottom .dropdown .dropdown-menu {
  top: auto;
  bottom: 100%;
  margin-bottom: 2px;
}
@media (min-width: 541px) {
  .navbar-right .dropdown-menu {
    left: auto;
    right: 0;
  }
  .navbar-right .dropdown-menu-left {
    left: 0;
    right: auto;
  }
}
.btn-group,
.btn-group-vertical {
  position: relative;
  display: inline-block;
  vertical-align: middle;
}
.btn-group > .btn,
.btn-group-vertical > .btn {
  position: relative;
  float: left;
}
.btn-group > .btn:hover,
.btn-group-vertical > .btn:hover,
.btn-group > .btn:focus,
.btn-group-vertical > .btn:focus,
.btn-group > .btn:active,
.btn-group-vertical > .btn:active,
.btn-group > .btn.active,
.btn-group-vertical > .btn.active {
  z-index: 2;
}
.btn-group .btn + .btn,
.btn-group .btn + .btn-group,
.btn-group .btn-group + .btn,
.btn-group .btn-group + .btn-group {
  margin-left: -1px;
}
.btn-toolbar {
  margin-left: -5px;
}
.btn-toolbar .btn,
.btn-toolbar .btn-group,
.btn-toolbar .input-group {
  float: left;
}
.btn-toolbar > .btn,
.btn-toolbar > .btn-group,
.btn-toolbar > .input-group {
  margin-left: 5px;
}
.btn-group > .btn:not(:first-child):not(:last-child):not(.dropdown-toggle) {
  border-radius: 0;
}
.btn-group > .btn:first-child {
  margin-left: 0;
}
.btn-group > .btn:first-child:not(:last-child):not(.dropdown-toggle) {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.btn-group > .btn:last-child:not(:first-child),
.btn-group > .dropdown-toggle:not(:first-child) {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.btn-group > .btn-group {
  float: left;
}
.btn-group > .btn-group:not(:first-child):not(:last-child) > .btn {
  border-radius: 0;
}
.btn-group > .btn-group:first-child:not(:last-child) > .btn:last-child,
.btn-group > .btn-group:first-child:not(:last-child) > .dropdown-toggle {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.btn-group > .btn-group:last-child:not(:first-child) > .btn:first-child {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.btn-group .dropdown-toggle:active,
.btn-group.open .dropdown-toggle {
  outline: 0;
}
.btn-group > .btn + .dropdown-toggle {
  padding-left: 8px;
  padding-right: 8px;
}
.btn-group > .btn-lg + .dropdown-toggle {
  padding-left: 12px;
  padding-right: 12px;
}
.btn-group.open .dropdown-toggle {
  -webkit-box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
  box-shadow: inset 0 3px 5px rgba(0, 0, 0, 0.125);
}
.btn-group.open .dropdown-toggle.btn-link {
  -webkit-box-shadow: none;
  box-shadow: none;
}
.btn .caret {
  margin-left: 0;
}
.btn-lg .caret {
  border-width: 5px 5px 0;
  border-bottom-width: 0;
}
.dropup .btn-lg .caret {
  border-width: 0 5px 5px;
}
.btn-group-vertical > .btn,
.btn-group-vertical > .btn-group,
.btn-group-vertical > .btn-group > .btn {
  display: block;
  float: none;
  width: 100%;
  max-width: 100%;
}
.btn-group-vertical > .btn-group > .btn {
  float: none;
}
.btn-group-vertical > .btn + .btn,
.btn-group-vertical > .btn + .btn-group,
.btn-group-vertical > .btn-group + .btn,
.btn-group-vertical > .btn-group + .btn-group {
  margin-top: -1px;
  margin-left: 0;
}
.btn-group-vertical > .btn:not(:first-child):not(:last-child) {
  border-radius: 0;
}
.btn-group-vertical > .btn:first-child:not(:last-child) {
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.btn-group-vertical > .btn:last-child:not(:first-child) {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}
.btn-group-vertical > .btn-group:not(:first-child):not(:last-child) > .btn {
  border-radius: 0;
}
.btn-group-vertical > .btn-group:first-child:not(:last-child) > .btn:last-child,
.btn-group-vertical > .btn-group:first-child:not(:last-child) > .dropdown-toggle {
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.btn-group-vertical > .btn-group:last-child:not(:first-child) > .btn:first-child {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.btn-group-justified {
  display: table;
  width: 100%;
  table-layout: fixed;
  border-collapse: separate;
}
.btn-group-justified > .btn,
.btn-group-justified > .btn-group {
  float: none;
  display: table-cell;
  width: 1%;
}
.btn-group-justified > .btn-group .btn {
  width: 100%;
}
.btn-group-justified > .btn-group .dropdown-menu {
  left: auto;
}
[data-toggle="buttons"] > .btn input[type="radio"],
[data-toggle="buttons"] > .btn-group > .btn input[type="radio"],
[data-toggle="buttons"] > .btn input[type="checkbox"],
[data-toggle="buttons"] > .btn-group > .btn input[type="checkbox"] {
  position: absolute;
  clip: rect(0, 0, 0, 0);
  pointer-events: none;
}
.input-group {
  position: relative;
  display: table;
  border-collapse: separate;
}
.input-group[class*="col-"] {
  float: none;
  padding-left: 0;
  padding-right: 0;
}
.input-group .form-control {
  position: relative;
  z-index: 2;
  float: left;
  width: 100%;
  margin-bottom: 0;
}
.input-group .form-control:focus {
  z-index: 3;
}
.input-group-lg > .form-control,
.input-group-lg > .input-group-addon,
.input-group-lg > .input-group-btn > .btn {
  height: 45px;
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
  border-radius: 3px;
}
select.input-group-lg > .form-control,
select.input-group-lg > .input-group-addon,
select.input-group-lg > .input-group-btn > .btn {
  height: 45px;
  line-height: 45px;
}
textarea.input-group-lg > .form-control,
textarea.input-group-lg > .input-group-addon,
textarea.input-group-lg > .input-group-btn > .btn,
select[multiple].input-group-lg > .form-control,
select[multiple].input-group-lg > .input-group-addon,
select[multiple].input-group-lg > .input-group-btn > .btn {
  height: auto;
}
.input-group-sm > .form-control,
.input-group-sm > .input-group-addon,
.input-group-sm > .input-group-btn > .btn {
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
}
select.input-group-sm > .form-control,
select.input-group-sm > .input-group-addon,
select.input-group-sm > .input-group-btn > .btn {
  height: 30px;
  line-height: 30px;
}
textarea.input-group-sm > .form-control,
textarea.input-group-sm > .input-group-addon,
textarea.input-group-sm > .input-group-btn > .btn,
select[multiple].input-group-sm > .form-control,
select[multiple].input-group-sm > .input-group-addon,
select[multiple].input-group-sm > .input-group-btn > .btn {
  height: auto;
}
.input-group-addon,
.input-group-btn,
.input-group .form-control {
  display: table-cell;
}
.input-group-addon:not(:first-child):not(:last-child),
.input-group-btn:not(:first-child):not(:last-child),
.input-group .form-control:not(:first-child):not(:last-child) {
  border-radius: 0;
}
.input-group-addon,
.input-group-btn {
  width: 1%;
  white-space: nowrap;
  vertical-align: middle;
}
.input-group-addon {
  padding: 6px 12px;
  font-size: 13px;
  font-weight: normal;
  line-height: 1;
  color: #555555;
  text-align: center;
  background-color: #eeeeee;
  border: 1px solid #ccc;
  border-radius: 2px;
}
.input-group-addon.input-sm {
  padding: 5px 10px;
  font-size: 12px;
  border-radius: 1px;
}
.input-group-addon.input-lg {
  padding: 10px 16px;
  font-size: 17px;
  border-radius: 3px;
}
.input-group-addon input[type="radio"],
.input-group-addon input[type="checkbox"] {
  margin-top: 0;
}
.input-group .form-control:first-child,
.input-group-addon:first-child,
.input-group-btn:first-child > .btn,
.input-group-btn:first-child > .btn-group > .btn,
.input-group-btn:first-child > .dropdown-toggle,
.input-group-btn:last-child > .btn:not(:last-child):not(.dropdown-toggle),
.input-group-btn:last-child > .btn-group:not(:last-child) > .btn {
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}
.input-group-addon:first-child {
  border-right: 0;
}
.input-group .form-control:last-child,
.input-group-addon:last-child,
.input-group-btn:last-child > .btn,
.input-group-btn:last-child > .btn-group > .btn,
.input-group-btn:last-child > .dropdown-toggle,
.input-group-btn:first-child > .btn:not(:first-child),
.input-group-btn:first-child > .btn-group:not(:first-child) > .btn {
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}
.input-group-addon:last-child {
  border-left: 0;
}
.input-group-btn {
  position: relative;
  font-size: 0;
  white-space: nowrap;
}
.input-group-btn > .btn {
  position: relative;
}
.input-group-btn > .btn + .btn {
  margin-left: -1px;
}
.input-group-btn > .btn:hover,
.input-group-btn > .btn:focus,
.input-group-btn > .btn:active {
  z-index: 2;
}
.input-group-btn:first-child > .btn,
.input-group-btn:first-child > .btn-group {
  margin-right: -1px;
}
.input-group-btn:last-child > .btn,
.input-group-btn:last-child > .btn-group {
  z-index: 2;
  margin-left: -1px;
}
.nav {
  margin-bottom: 0;
  padding-left: 0;
  list-style: none;
}
.nav > li {
  position: relative;
  display: block;
}
.nav > li > a {
  position: relative;
  display: block;
  padding: 10px 15px;
}
.nav > li > a:hover,
.nav > li > a:focus {
  text-decoration: none;
  background-color: #eeeeee;
}
.nav > li.disabled > a {
  color: #777777;
}
.nav > li.disabled > a:hover,
.nav > li.disabled > a:focus {
  color: #777777;
  text-decoration: none;
  background-color: transparent;
  cursor: not-allowed;
}
.nav .open > a,
.nav .open > a:hover,
.nav .open > a:focus {
  background-color: #eeeeee;
  border-color: #337ab7;
}
.nav .nav-divider {
  height: 1px;
  margin: 8px 0;
  overflow: hidden;
  background-color: #e5e5e5;
}
.nav > li > a > img {
  max-width: none;
}
.nav-tabs {
  border-bottom: 1px solid #ddd;
}
.nav-tabs > li {
  float: left;
  margin-bottom: -1px;
}
.nav-tabs > li > a {
  margin-right: 2px;
  line-height: 1.42857143;
  border: 1px solid transparent;
  border-radius: 2px 2px 0 0;
}
.nav-tabs > li > a:hover {
  border-color: #eeeeee #eeeeee #ddd;
}
.nav-tabs > li.active > a,
.nav-tabs > li.active > a:hover,
.nav-tabs > li.active > a:focus {
  color: #555555;
  background-color: #fff;
  border: 1px solid #ddd;
  border-bottom-color: transparent;
  cursor: default;
}
.nav-tabs.nav-justified {
  width: 100%;
  border-bottom: 0;
}
.nav-tabs.nav-justified > li {
  float: none;
}
.nav-tabs.nav-justified > li > a {
  text-align: center;
  margin-bottom: 5px;
}
.nav-tabs.nav-justified > .dropdown .dropdown-menu {
  top: auto;
  left: auto;
}
@media (min-width: 768px) {
  .nav-tabs.nav-justified > li {
    display: table-cell;
    width: 1%;
  }
  .nav-tabs.nav-justified > li > a {
    margin-bottom: 0;
  }
}
.nav-tabs.nav-justified > li > a {
  margin-right: 0;
  border-radius: 2px;
}
.nav-tabs.nav-justified > .active > a,
.nav-tabs.nav-justified > .active > a:hover,
.nav-tabs.nav-justified > .active > a:focus {
  border: 1px solid #ddd;
}
@media (min-width: 768px) {
  .nav-tabs.nav-justified > li > a {
    border-bottom: 1px solid #ddd;
    border-radius: 2px 2px 0 0;
  }
  .nav-tabs.nav-justified > .active > a,
  .nav-tabs.nav-justified > .active > a:hover,
  .nav-tabs.nav-justified > .active > a:focus {
    border-bottom-color: #fff;
  }
}
.nav-pills > li {
  float: left;
}
.nav-pills > li > a {
  border-radius: 2px;
}
.nav-pills > li + li {
  margin-left: 2px;
}
.nav-pills > li.active > a,
.nav-pills > li.active > a:hover,
.nav-pills > li.active > a:focus {
  color: #fff;
  background-color: #337ab7;
}
.nav-stacked > li {
  float: none;
}
.nav-stacked > li + li {
  margin-top: 2px;
  margin-left: 0;
}
.nav-justified {
  width: 100%;
}
.nav-justified > li {
  float: none;
}
.nav-justified > li > a {
  text-align: center;
  margin-bottom: 5px;
}
.nav-justified > .dropdown .dropdown-menu {
  top: auto;
  left: auto;
}
@media (min-width: 768px) {
  .nav-justified > li {
    display: table-cell;
    width: 1%;
  }
  .nav-justified > li > a {
    margin-bottom: 0;
  }
}
.nav-tabs-justified {
  border-bottom: 0;
}
.nav-tabs-justified > li > a {
  margin-right: 0;
  border-radius: 2px;
}
.nav-tabs-justified > .active > a,
.nav-tabs-justified > .active > a:hover,
.nav-tabs-justified > .active > a:focus {
  border: 1px solid #ddd;
}
@media (min-width: 768px) {
  .nav-tabs-justified > li > a {
    border-bottom: 1px solid #ddd;
    border-radius: 2px 2px 0 0;
  }
  .nav-tabs-justified > .active > a,
  .nav-tabs-justified > .active > a:hover,
  .nav-tabs-justified > .active > a:focus {
    border-bottom-color: #fff;
  }
}
.tab-content > .tab-pane {
  display: none;
}
.tab-content > .active {
  display: block;
}
.nav-tabs .dropdown-menu {
  margin-top: -1px;
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.navbar {
  position: relative;
  min-height: 30px;
  margin-bottom: 18px;
  border: 1px solid transparent;
}
@media (min-width: 541px) {
  .navbar {
    border-radius: 2px;
  }
}
@media (min-width: 541px) {
  .navbar-header {
    float: left;
  }
}
.navbar-collapse {
  overflow-x: visible;
  padding-right: 0px;
  padding-left: 0px;
  border-top: 1px solid transparent;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1);
  -webkit-overflow-scrolling: touch;
}
.navbar-collapse.in {
  overflow-y: auto;
}
@media (min-width: 541px) {
  .navbar-collapse {
    width: auto;
    border-top: 0;
    box-shadow: none;
  }
  .navbar-collapse.collapse {
    display: block !important;
    height: auto !important;
    padding-bottom: 0;
    overflow: visible !important;
  }
  .navbar-collapse.in {
    overflow-y: visible;
  }
  .navbar-fixed-top .navbar-collapse,
  .navbar-static-top .navbar-collapse,
  .navbar-fixed-bottom .navbar-collapse {
    padding-left: 0;
    padding-right: 0;
  }
}
.navbar-fixed-top .navbar-collapse,
.navbar-fixed-bottom .navbar-collapse {
  max-height: 340px;
}
@media (max-device-width: 540px) and (orientation: landscape) {
  .navbar-fixed-top .navbar-collapse,
  .navbar-fixed-bottom .navbar-collapse {
    max-height: 200px;
  }
}
.container > .navbar-header,
.container-fluid > .navbar-header,
.container > .navbar-collapse,
.container-fluid > .navbar-collapse {
  margin-right: 0px;
  margin-left: 0px;
}
@media (min-width: 541px) {
  .container > .navbar-header,
  .container-fluid > .navbar-header,
  .container > .navbar-collapse,
  .container-fluid > .navbar-collapse {
    margin-right: 0;
    margin-left: 0;
  }
}
.navbar-static-top {
  z-index: 1000;
  border-width: 0 0 1px;
}
@media (min-width: 541px) {
  .navbar-static-top {
    border-radius: 0;
  }
}
.navbar-fixed-top,
.navbar-fixed-bottom {
  position: fixed;
  right: 0;
  left: 0;
  z-index: 1030;
}
@media (min-width: 541px) {
  .navbar-fixed-top,
  .navbar-fixed-bottom {
    border-radius: 0;
  }
}
.navbar-fixed-top {
  top: 0;
  border-width: 0 0 1px;
}
.navbar-fixed-bottom {
  bottom: 0;
  margin-bottom: 0;
  border-width: 1px 0 0;
}
.navbar-brand {
  float: left;
  padding: 6px 0px;
  font-size: 17px;
  line-height: 18px;
  height: 30px;
}
.navbar-brand:hover,
.navbar-brand:focus {
  text-decoration: none;
}
.navbar-brand > img {
  display: block;
}
@media (min-width: 541px) {
  .navbar > .container .navbar-brand,
  .navbar > .container-fluid .navbar-brand {
    margin-left: 0px;
  }
}
.navbar-toggle {
  position: relative;
  float: right;
  margin-right: 0px;
  padding: 9px 10px;
  margin-top: -2px;
  margin-bottom: -2px;
  background-color: transparent;
  background-image: none;
  border: 1px solid transparent;
  border-radius: 2px;
}
.navbar-toggle:focus {
  outline: 0;
}
.navbar-toggle .icon-bar {
  display: block;
  width: 22px;
  height: 2px;
  border-radius: 1px;
}
.navbar-toggle .icon-bar + .icon-bar {
  margin-top: 4px;
}
@media (min-width: 541px) {
  .navbar-toggle {
    display: none;
  }
}
.navbar-nav {
  margin: 3px 0px;
}
.navbar-nav > li > a {
  padding-top: 10px;
  padding-bottom: 10px;
  line-height: 18px;
}
@media (max-width: 540px) {
  .navbar-nav .open .dropdown-menu {
    position: static;
    float: none;
    width: auto;
    margin-top: 0;
    background-color: transparent;
    border: 0;
    box-shadow: none;
  }
  .navbar-nav .open .dropdown-menu > li > a,
  .navbar-nav .open .dropdown-menu .dropdown-header {
    padding: 5px 15px 5px 25px;
  }
  .navbar-nav .open .dropdown-menu > li > a {
    line-height: 18px;
  }
  .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-nav .open .dropdown-menu > li > a:focus {
    background-image: none;
  }
}
@media (min-width: 541px) {
  .navbar-nav {
    float: left;
    margin: 0;
  }
  .navbar-nav > li {
    float: left;
  }
  .navbar-nav > li > a {
    padding-top: 6px;
    padding-bottom: 6px;
  }
}
.navbar-form {
  margin-left: 0px;
  margin-right: 0px;
  padding: 10px 0px;
  border-top: 1px solid transparent;
  border-bottom: 1px solid transparent;
  -webkit-box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1), 0 1px 0 rgba(255, 255, 255, 0.1);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.1), 0 1px 0 rgba(255, 255, 255, 0.1);
  margin-top: -1px;
  margin-bottom: -1px;
}
@media (min-width: 768px) {
  .navbar-form .form-group {
    display: inline-block;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .form-control {
    display: inline-block;
    width: auto;
    vertical-align: middle;
  }
  .navbar-form .form-control-static {
    display: inline-block;
  }
  .navbar-form .input-group {
    display: inline-table;
    vertical-align: middle;
  }
  .navbar-form .input-group .input-group-addon,
  .navbar-form .input-group .input-group-btn,
  .navbar-form .input-group .form-control {
    width: auto;
  }
  .navbar-form .input-group > .form-control {
    width: 100%;
  }
  .navbar-form .control-label {
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .radio,
  .navbar-form .checkbox {
    display: inline-block;
    margin-top: 0;
    margin-bottom: 0;
    vertical-align: middle;
  }
  .navbar-form .radio label,
  .navbar-form .checkbox label {
    padding-left: 0;
  }
  .navbar-form .radio input[type="radio"],
  .navbar-form .checkbox input[type="checkbox"] {
    position: relative;
    margin-left: 0;
  }
  .navbar-form .has-feedback .form-control-feedback {
    top: 0;
  }
}
@media (max-width: 540px) {
  .navbar-form .form-group {
    margin-bottom: 5px;
  }
  .navbar-form .form-group:last-child {
    margin-bottom: 0;
  }
}
@media (min-width: 541px) {
  .navbar-form {
    width: auto;
    border: 0;
    margin-left: 0;
    margin-right: 0;
    padding-top: 0;
    padding-bottom: 0;
    -webkit-box-shadow: none;
    box-shadow: none;
  }
}
.navbar-nav > li > .dropdown-menu {
  margin-top: 0;
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.navbar-fixed-bottom .navbar-nav > li > .dropdown-menu {
  margin-bottom: 0;
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.navbar-btn {
  margin-top: -1px;
  margin-bottom: -1px;
}
.navbar-btn.btn-sm {
  margin-top: 0px;
  margin-bottom: 0px;
}
.navbar-btn.btn-xs {
  margin-top: 4px;
  margin-bottom: 4px;
}
.navbar-text {
  margin-top: 6px;
  margin-bottom: 6px;
}
@media (min-width: 541px) {
  .navbar-text {
    float: left;
    margin-left: 0px;
    margin-right: 0px;
  }
}
@media (min-width: 541px) {
  .navbar-left {
    float: left !important;
    float: left;
  }
  .navbar-right {
    float: right !important;
    float: right;
    margin-right: 0px;
  }
  .navbar-right ~ .navbar-right {
    margin-right: 0;
  }
}
.navbar-default {
  background-color: #f8f8f8;
  border-color: #e7e7e7;
}
.navbar-default .navbar-brand {
  color: #777;
}
.navbar-default .navbar-brand:hover,
.navbar-default .navbar-brand:focus {
  color: #5e5e5e;
  background-color: transparent;
}
.navbar-default .navbar-text {
  color: #777;
}
.navbar-default .navbar-nav > li > a {
  color: #777;
}
.navbar-default .navbar-nav > li > a:hover,
.navbar-default .navbar-nav > li > a:focus {
  color: #333;
  background-color: transparent;
}
.navbar-default .navbar-nav > .active > a,
.navbar-default .navbar-nav > .active > a:hover,
.navbar-default .navbar-nav > .active > a:focus {
  color: #555;
  background-color: #e7e7e7;
}
.navbar-default .navbar-nav > .disabled > a,
.navbar-default .navbar-nav > .disabled > a:hover,
.navbar-default .navbar-nav > .disabled > a:focus {
  color: #ccc;
  background-color: transparent;
}
.navbar-default .navbar-toggle {
  border-color: #ddd;
}
.navbar-default .navbar-toggle:hover,
.navbar-default .navbar-toggle:focus {
  background-color: #ddd;
}
.navbar-default .navbar-toggle .icon-bar {
  background-color: #888;
}
.navbar-default .navbar-collapse,
.navbar-default .navbar-form {
  border-color: #e7e7e7;
}
.navbar-default .navbar-nav > .open > a,
.navbar-default .navbar-nav > .open > a:hover,
.navbar-default .navbar-nav > .open > a:focus {
  background-color: #e7e7e7;
  color: #555;
}
@media (max-width: 540px) {
  .navbar-default .navbar-nav .open .dropdown-menu > li > a {
    color: #777;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > li > a:focus {
    color: #333;
    background-color: transparent;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a,
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > .active > a:focus {
    color: #555;
    background-color: #e7e7e7;
  }
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a,
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a:hover,
  .navbar-default .navbar-nav .open .dropdown-menu > .disabled > a:focus {
    color: #ccc;
    background-color: transparent;
  }
}
.navbar-default .navbar-link {
  color: #777;
}
.navbar-default .navbar-link:hover {
  color: #333;
}
.navbar-default .btn-link {
  color: #777;
}
.navbar-default .btn-link:hover,
.navbar-default .btn-link:focus {
  color: #333;
}
.navbar-default .btn-link[disabled]:hover,
fieldset[disabled] .navbar-default .btn-link:hover,
.navbar-default .btn-link[disabled]:focus,
fieldset[disabled] .navbar-default .btn-link:focus {
  color: #ccc;
}
.navbar-inverse {
  background-color: #222;
  border-color: #080808;
}
.navbar-inverse .navbar-brand {
  color: #9d9d9d;
}
.navbar-inverse .navbar-brand:hover,
.navbar-inverse .navbar-brand:focus {
  color: #fff;
  background-color: transparent;
}
.navbar-inverse .navbar-text {
  color: #9d9d9d;
}
.navbar-inverse .navbar-nav > li > a {
  color: #9d9d9d;
}
.navbar-inverse .navbar-nav > li > a:hover,
.navbar-inverse .navbar-nav > li > a:focus {
  color: #fff;
  background-color: transparent;
}
.navbar-inverse .navbar-nav > .active > a,
.navbar-inverse .navbar-nav > .active > a:hover,
.navbar-inverse .navbar-nav > .active > a:focus {
  color: #fff;
  background-color: #080808;
}
.navbar-inverse .navbar-nav > .disabled > a,
.navbar-inverse .navbar-nav > .disabled > a:hover,
.navbar-inverse .navbar-nav > .disabled > a:focus {
  color: #444;
  background-color: transparent;
}
.navbar-inverse .navbar-toggle {
  border-color: #333;
}
.navbar-inverse .navbar-toggle:hover,
.navbar-inverse .navbar-toggle:focus {
  background-color: #333;
}
.navbar-inverse .navbar-toggle .icon-bar {
  background-color: #fff;
}
.navbar-inverse .navbar-collapse,
.navbar-inverse .navbar-form {
  border-color: #101010;
}
.navbar-inverse .navbar-nav > .open > a,
.navbar-inverse .navbar-nav > .open > a:hover,
.navbar-inverse .navbar-nav > .open > a:focus {
  background-color: #080808;
  color: #fff;
}
@media (max-width: 540px) {
  .navbar-inverse .navbar-nav .open .dropdown-menu > .dropdown-header {
    border-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu .divider {
    background-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a {
    color: #9d9d9d;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > li > a:focus {
    color: #fff;
    background-color: transparent;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .active > a:focus {
    color: #fff;
    background-color: #080808;
  }
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a:hover,
  .navbar-inverse .navbar-nav .open .dropdown-menu > .disabled > a:focus {
    color: #444;
    background-color: transparent;
  }
}
.navbar-inverse .navbar-link {
  color: #9d9d9d;
}
.navbar-inverse .navbar-link:hover {
  color: #fff;
}
.navbar-inverse .btn-link {
  color: #9d9d9d;
}
.navbar-inverse .btn-link:hover,
.navbar-inverse .btn-link:focus {
  color: #fff;
}
.navbar-inverse .btn-link[disabled]:hover,
fieldset[disabled] .navbar-inverse .btn-link:hover,
.navbar-inverse .btn-link[disabled]:focus,
fieldset[disabled] .navbar-inverse .btn-link:focus {
  color: #444;
}
.breadcrumb {
  padding: 8px 15px;
  margin-bottom: 18px;
  list-style: none;
  background-color: #f5f5f5;
  border-radius: 2px;
}
.breadcrumb > li {
  display: inline-block;
}
.breadcrumb > li + li:before {
  content: "/\00a0";
  padding: 0 5px;
  color: #5e5e5e;
}
.breadcrumb > .active {
  color: #777777;
}
.pagination {
  display: inline-block;
  padding-left: 0;
  margin: 18px 0;
  border-radius: 2px;
}
.pagination > li {
  display: inline;
}
.pagination > li > a,
.pagination > li > span {
  position: relative;
  float: left;
  padding: 6px 12px;
  line-height: 1.42857143;
  text-decoration: none;
  color: #337ab7;
  background-color: #fff;
  border: 1px solid #ddd;
  margin-left: -1px;
}
.pagination > li:first-child > a,
.pagination > li:first-child > span {
  margin-left: 0;
  border-bottom-left-radius: 2px;
  border-top-left-radius: 2px;
}
.pagination > li:last-child > a,
.pagination > li:last-child > span {
  border-bottom-right-radius: 2px;
  border-top-right-radius: 2px;
}
.pagination > li > a:hover,
.pagination > li > span:hover,
.pagination > li > a:focus,
.pagination > li > span:focus {
  z-index: 2;
  color: #23527c;
  background-color: #eeeeee;
  border-color: #ddd;
}
.pagination > .active > a,
.pagination > .active > span,
.pagination > .active > a:hover,
.pagination > .active > span:hover,
.pagination > .active > a:focus,
.pagination > .active > span:focus {
  z-index: 3;
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
  cursor: default;
}
.pagination > .disabled > span,
.pagination > .disabled > span:hover,
.pagination > .disabled > span:focus,
.pagination > .disabled > a,
.pagination > .disabled > a:hover,
.pagination > .disabled > a:focus {
  color: #777777;
  background-color: #fff;
  border-color: #ddd;
  cursor: not-allowed;
}
.pagination-lg > li > a,
.pagination-lg > li > span {
  padding: 10px 16px;
  font-size: 17px;
  line-height: 1.3333333;
}
.pagination-lg > li:first-child > a,
.pagination-lg > li:first-child > span {
  border-bottom-left-radius: 3px;
  border-top-left-radius: 3px;
}
.pagination-lg > li:last-child > a,
.pagination-lg > li:last-child > span {
  border-bottom-right-radius: 3px;
  border-top-right-radius: 3px;
}
.pagination-sm > li > a,
.pagination-sm > li > span {
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
}
.pagination-sm > li:first-child > a,
.pagination-sm > li:first-child > span {
  border-bottom-left-radius: 1px;
  border-top-left-radius: 1px;
}
.pagination-sm > li:last-child > a,
.pagination-sm > li:last-child > span {
  border-bottom-right-radius: 1px;
  border-top-right-radius: 1px;
}
.pager {
  padding-left: 0;
  margin: 18px 0;
  list-style: none;
  text-align: center;
}
.pager li {
  display: inline;
}
.pager li > a,
.pager li > span {
  display: inline-block;
  padding: 5px 14px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 15px;
}
.pager li > a:hover,
.pager li > a:focus {
  text-decoration: none;
  background-color: #eeeeee;
}
.pager .next > a,
.pager .next > span {
  float: right;
}
.pager .previous > a,
.pager .previous > span {
  float: left;
}
.pager .disabled > a,
.pager .disabled > a:hover,
.pager .disabled > a:focus,
.pager .disabled > span {
  color: #777777;
  background-color: #fff;
  cursor: not-allowed;
}
.label {
  display: inline;
  padding: .2em .6em .3em;
  font-size: 75%;
  font-weight: bold;
  line-height: 1;
  color: #fff;
  text-align: center;
  white-space: nowrap;
  vertical-align: baseline;
  border-radius: .25em;
}
a.label:hover,
a.label:focus {
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
.label:empty {
  display: none;
}
.btn .label {
  position: relative;
  top: -1px;
}
.label-default {
  background-color: #777777;
}
.label-default[href]:hover,
.label-default[href]:focus {
  background-color: #5e5e5e;
}
.label-primary {
  background-color: #337ab7;
}
.label-primary[href]:hover,
.label-primary[href]:focus {
  background-color: #286090;
}
.label-success {
  background-color: #5cb85c;
}
.label-success[href]:hover,
.label-success[href]:focus {
  background-color: #449d44;
}
.label-info {
  background-color: #5bc0de;
}
.label-info[href]:hover,
.label-info[href]:focus {
  background-color: #31b0d5;
}
.label-warning {
  background-color: #f0ad4e;
}
.label-warning[href]:hover,
.label-warning[href]:focus {
  background-color: #ec971f;
}
.label-danger {
  background-color: #d9534f;
}
.label-danger[href]:hover,
.label-danger[href]:focus {
  background-color: #c9302c;
}
.badge {
  display: inline-block;
  min-width: 10px;
  padding: 3px 7px;
  font-size: 12px;
  font-weight: bold;
  color: #fff;
  line-height: 1;
  vertical-align: middle;
  white-space: nowrap;
  text-align: center;
  background-color: #777777;
  border-radius: 10px;
}
.badge:empty {
  display: none;
}
.btn .badge {
  position: relative;
  top: -1px;
}
.btn-xs .badge,
.btn-group-xs > .btn .badge {
  top: 0;
  padding: 1px 5px;
}
a.badge:hover,
a.badge:focus {
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}
.list-group-item.active > .badge,
.nav-pills > .active > a > .badge {
  color: #337ab7;
  background-color: #fff;
}
.list-group-item > .badge {
  float: right;
}
.list-group-item > .badge + .badge {
  margin-right: 5px;
}
.nav-pills > li > a > .badge {
  margin-left: 3px;
}
.jumbotron {
  padding-top: 30px;
  padding-bottom: 30px;
  margin-bottom: 30px;
  color: inherit;
  background-color: #eeeeee;
}
.jumbotron h1,
.jumbotron .h1 {
  color: inherit;
}
.jumbotron p {
  margin-bottom: 15px;
  font-size: 20px;
  font-weight: 200;
}
.jumbotron > hr {
  border-top-color: #d5d5d5;
}
.container .jumbotron,
.container-fluid .jumbotron {
  border-radius: 3px;
  padding-left: 0px;
  padding-right: 0px;
}
.jumbotron .container {
  max-width: 100%;
}
@media screen and (min-width: 768px) {
  .jumbotron {
    padding-top: 48px;
    padding-bottom: 48px;
  }
  .container .jumbotron,
  .container-fluid .jumbotron {
    padding-left: 60px;
    padding-right: 60px;
  }
  .jumbotron h1,
  .jumbotron .h1 {
    font-size: 59px;
  }
}
.thumbnail {
  display: block;
  padding: 4px;
  margin-bottom: 18px;
  line-height: 1.42857143;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 2px;
  -webkit-transition: border 0.2s ease-in-out;
  -o-transition: border 0.2s ease-in-out;
  transition: border 0.2s ease-in-out;
}
.thumbnail > img,
.thumbnail a > img {
  margin-left: auto;
  margin-right: auto;
}
a.thumbnail:hover,
a.thumbnail:focus,
a.thumbnail.active {
  border-color: #337ab7;
}
.thumbnail .caption {
  padding: 9px;
  color: #000;
}
.alert {
  padding: 15px;
  margin-bottom: 18px;
  border: 1px solid transparent;
  border-radius: 2px;
}
.alert h4 {
  margin-top: 0;
  color: inherit;
}
.alert .alert-link {
  font-weight: bold;
}
.alert > p,
.alert > ul {
  margin-bottom: 0;
}
.alert > p + p {
  margin-top: 5px;
}
.alert-dismissable,
.alert-dismissible {
  padding-right: 35px;
}
.alert-dismissable .close,
.alert-dismissible .close {
  position: relative;
  top: -2px;
  right: -21px;
  color: inherit;
}
.alert-success {
  background-color: #dff0d8;
  border-color: #d6e9c6;
  color: #3c763d;
}
.alert-success hr {
  border-top-color: #c9e2b3;
}
.alert-success .alert-link {
  color: #2b542c;
}
.alert-info {
  background-color: #d9edf7;
  border-color: #bce8f1;
  color: #31708f;
}
.alert-info hr {
  border-top-color: #a6e1ec;
}
.alert-info .alert-link {
  color: #245269;
}
.alert-warning {
  background-color: #fcf8e3;
  border-color: #faebcc;
  color: #8a6d3b;
}
.alert-warning hr {
  border-top-color: #f7e1b5;
}
.alert-warning .alert-link {
  color: #66512c;
}
.alert-danger {
  background-color: #f2dede;
  border-color: #ebccd1;
  color: #a94442;
}
.alert-danger hr {
  border-top-color: #e4b9c0;
}
.alert-danger .alert-link {
  color: #843534;
}
@-webkit-keyframes progress-bar-stripes {
  from {
    background-position: 40px 0;
  }
  to {
    background-position: 0 0;
  }
}
@keyframes progress-bar-stripes {
  from {
    background-position: 40px 0;
  }
  to {
    background-position: 0 0;
  }
}
.progress {
  overflow: hidden;
  height: 18px;
  margin-bottom: 18px;
  background-color: #f5f5f5;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.1);
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.1);
}
.progress-bar {
  float: left;
  width: 0%;
  height: 100%;
  font-size: 12px;
  line-height: 18px;
  color: #fff;
  text-align: center;
  background-color: #337ab7;
  -webkit-box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.15);
  box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.15);
  -webkit-transition: width 0.6s ease;
  -o-transition: width 0.6s ease;
  transition: width 0.6s ease;
}
.progress-striped .progress-bar,
.progress-bar-striped {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-size: 40px 40px;
}
.progress.active .progress-bar,
.progress-bar.active {
  -webkit-animation: progress-bar-stripes 2s linear infinite;
  -o-animation: progress-bar-stripes 2s linear infinite;
  animation: progress-bar-stripes 2s linear infinite;
}
.progress-bar-success {
  background-color: #5cb85c;
}
.progress-striped .progress-bar-success {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-info {
  background-color: #5bc0de;
}
.progress-striped .progress-bar-info {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-warning {
  background-color: #f0ad4e;
}
.progress-striped .progress-bar-warning {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.progress-bar-danger {
  background-color: #d9534f;
}
.progress-striped .progress-bar-danger {
  background-image: -webkit-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: -o-linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.15) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.15) 50%, rgba(255, 255, 255, 0.15) 75%, transparent 75%, transparent);
}
.media {
  margin-top: 15px;
}
.media:first-child {
  margin-top: 0;
}
.media,
.media-body {
  zoom: 1;
  overflow: hidden;
}
.media-body {
  width: 10000px;
}
.media-object {
  display: block;
}
.media-object.img-thumbnail {
  max-width: none;
}
.media-right,
.media > .pull-right {
  padding-left: 10px;
}
.media-left,
.media > .pull-left {
  padding-right: 10px;
}
.media-left,
.media-right,
.media-body {
  display: table-cell;
  vertical-align: top;
}
.media-middle {
  vertical-align: middle;
}
.media-bottom {
  vertical-align: bottom;
}
.media-heading {
  margin-top: 0;
  margin-bottom: 5px;
}
.media-list {
  padding-left: 0;
  list-style: none;
}
.list-group {
  margin-bottom: 20px;
  padding-left: 0;
}
.list-group-item {
  position: relative;
  display: block;
  padding: 10px 15px;
  margin-bottom: -1px;
  background-color: #fff;
  border: 1px solid #ddd;
}
.list-group-item:first-child {
  border-top-right-radius: 2px;
  border-top-left-radius: 2px;
}
.list-group-item:last-child {
  margin-bottom: 0;
  border-bottom-right-radius: 2px;
  border-bottom-left-radius: 2px;
}
a.list-group-item,
button.list-group-item {
  color: #555;
}
a.list-group-item .list-group-item-heading,
button.list-group-item .list-group-item-heading {
  color: #333;
}
a.list-group-item:hover,
button.list-group-item:hover,
a.list-group-item:focus,
button.list-group-item:focus {
  text-decoration: none;
  color: #555;
  background-color: #f5f5f5;
}
button.list-group-item {
  width: 100%;
  text-align: left;
}
.list-group-item.disabled,
.list-group-item.disabled:hover,
.list-group-item.disabled:focus {
  background-color: #eeeeee;
  color: #777777;
  cursor: not-allowed;
}
.list-group-item.disabled .list-group-item-heading,
.list-group-item.disabled:hover .list-group-item-heading,
.list-group-item.disabled:focus .list-group-item-heading {
  color: inherit;
}
.list-group-item.disabled .list-group-item-text,
.list-group-item.disabled:hover .list-group-item-text,
.list-group-item.disabled:focus .list-group-item-text {
  color: #777777;
}
.list-group-item.active,
.list-group-item.active:hover,
.list-group-item.active:focus {
  z-index: 2;
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
}
.list-group-item.active .list-group-item-heading,
.list-group-item.active:hover .list-group-item-heading,
.list-group-item.active:focus .list-group-item-heading,
.list-group-item.active .list-group-item-heading > small,
.list-group-item.active:hover .list-group-item-heading > small,
.list-group-item.active:focus .list-group-item-heading > small,
.list-group-item.active .list-group-item-heading > .small,
.list-group-item.active:hover .list-group-item-heading > .small,
.list-group-item.active:focus .list-group-item-heading > .small {
  color: inherit;
}
.list-group-item.active .list-group-item-text,
.list-group-item.active:hover .list-group-item-text,
.list-group-item.active:focus .list-group-item-text {
  color: #c7ddef;
}
.list-group-item-success {
  color: #3c763d;
  background-color: #dff0d8;
}
a.list-group-item-success,
button.list-group-item-success {
  color: #3c763d;
}
a.list-group-item-success .list-group-item-heading,
button.list-group-item-success .list-group-item-heading {
  color: inherit;
}
a.list-group-item-success:hover,
button.list-group-item-success:hover,
a.list-group-item-success:focus,
button.list-group-item-success:focus {
  color: #3c763d;
  background-color: #d0e9c6;
}
a.list-group-item-success.active,
button.list-group-item-success.active,
a.list-group-item-success.active:hover,
button.list-group-item-success.active:hover,
a.list-group-item-success.active:focus,
button.list-group-item-success.active:focus {
  color: #fff;
  background-color: #3c763d;
  border-color: #3c763d;
}
.list-group-item-info {
  color: #31708f;
  background-color: #d9edf7;
}
a.list-group-item-info,
button.list-group-item-info {
  color: #31708f;
}
a.list-group-item-info .list-group-item-heading,
button.list-group-item-info .list-group-item-heading {
  color: inherit;
}
a.list-group-item-info:hover,
button.list-group-item-info:hover,
a.list-group-item-info:focus,
button.list-group-item-info:focus {
  color: #31708f;
  background-color: #c4e3f3;
}
a.list-group-item-info.active,
button.list-group-item-info.active,
a.list-group-item-info.active:hover,
button.list-group-item-info.active:hover,
a.list-group-item-info.active:focus,
button.list-group-item-info.active:focus {
  color: #fff;
  background-color: #31708f;
  border-color: #31708f;
}
.list-group-item-warning {
  color: #8a6d3b;
  background-color: #fcf8e3;
}
a.list-group-item-warning,
button.list-group-item-warning {
  color: #8a6d3b;
}
a.list-group-item-warning .list-group-item-heading,
button.list-group-item-warning .list-group-item-heading {
  color: inherit;
}
a.list-group-item-warning:hover,
button.list-group-item-warning:hover,
a.list-group-item-warning:focus,
button.list-group-item-warning:focus {
  color: #8a6d3b;
  background-color: #faf2cc;
}
a.list-group-item-warning.active,
button.list-group-item-warning.active,
a.list-group-item-warning.active:hover,
button.list-group-item-warning.active:hover,
a.list-group-item-warning.active:focus,
button.list-group-item-warning.active:focus {
  color: #fff;
  background-color: #8a6d3b;
  border-color: #8a6d3b;
}
.list-group-item-danger {
  color: #a94442;
  background-color: #f2dede;
}
a.list-group-item-danger,
button.list-group-item-danger {
  color: #a94442;
}
a.list-group-item-danger .list-group-item-heading,
button.list-group-item-danger .list-group-item-heading {
  color: inherit;
}
a.list-group-item-danger:hover,
button.list-group-item-danger:hover,
a.list-group-item-danger:focus,
button.list-group-item-danger:focus {
  color: #a94442;
  background-color: #ebcccc;
}
a.list-group-item-danger.active,
button.list-group-item-danger.active,
a.list-group-item-danger.active:hover,
button.list-group-item-danger.active:hover,
a.list-group-item-danger.active:focus,
button.list-group-item-danger.active:focus {
  color: #fff;
  background-color: #a94442;
  border-color: #a94442;
}
.list-group-item-heading {
  margin-top: 0;
  margin-bottom: 5px;
}
.list-group-item-text {
  margin-bottom: 0;
  line-height: 1.3;
}
.panel {
  margin-bottom: 18px;
  background-color: #fff;
  border: 1px solid transparent;
  border-radius: 2px;
  -webkit-box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
}
.panel-body {
  padding: 15px;
}
.panel-heading {
  padding: 10px 15px;
  border-bottom: 1px solid transparent;
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel-heading > .dropdown .dropdown-toggle {
  color: inherit;
}
.panel-title {
  margin-top: 0;
  margin-bottom: 0;
  font-size: 15px;
  color: inherit;
}
.panel-title > a,
.panel-title > small,
.panel-title > .small,
.panel-title > small > a,
.panel-title > .small > a {
  color: inherit;
}
.panel-footer {
  padding: 10px 15px;
  background-color: #f5f5f5;
  border-top: 1px solid #ddd;
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .list-group,
.panel > .panel-collapse > .list-group {
  margin-bottom: 0;
}
.panel > .list-group .list-group-item,
.panel > .panel-collapse > .list-group .list-group-item {
  border-width: 1px 0;
  border-radius: 0;
}
.panel > .list-group:first-child .list-group-item:first-child,
.panel > .panel-collapse > .list-group:first-child .list-group-item:first-child {
  border-top: 0;
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel > .list-group:last-child .list-group-item:last-child,
.panel > .panel-collapse > .list-group:last-child .list-group-item:last-child {
  border-bottom: 0;
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .panel-heading + .panel-collapse > .list-group .list-group-item:first-child {
  border-top-right-radius: 0;
  border-top-left-radius: 0;
}
.panel-heading + .list-group .list-group-item:first-child {
  border-top-width: 0;
}
.list-group + .panel-footer {
  border-top-width: 0;
}
.panel > .table,
.panel > .table-responsive > .table,
.panel > .panel-collapse > .table {
  margin-bottom: 0;
}
.panel > .table caption,
.panel > .table-responsive > .table caption,
.panel > .panel-collapse > .table caption {
  padding-left: 15px;
  padding-right: 15px;
}
.panel > .table:first-child,
.panel > .table-responsive:first-child > .table:first-child {
  border-top-right-radius: 1px;
  border-top-left-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child {
  border-top-left-radius: 1px;
  border-top-right-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child td:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child td:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child td:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child td:first-child,
.panel > .table:first-child > thead:first-child > tr:first-child th:first-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child th:first-child,
.panel > .table:first-child > tbody:first-child > tr:first-child th:first-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child th:first-child {
  border-top-left-radius: 1px;
}
.panel > .table:first-child > thead:first-child > tr:first-child td:last-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child td:last-child,
.panel > .table:first-child > tbody:first-child > tr:first-child td:last-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child td:last-child,
.panel > .table:first-child > thead:first-child > tr:first-child th:last-child,
.panel > .table-responsive:first-child > .table:first-child > thead:first-child > tr:first-child th:last-child,
.panel > .table:first-child > tbody:first-child > tr:first-child th:last-child,
.panel > .table-responsive:first-child > .table:first-child > tbody:first-child > tr:first-child th:last-child {
  border-top-right-radius: 1px;
}
.panel > .table:last-child,
.panel > .table-responsive:last-child > .table:last-child {
  border-bottom-right-radius: 1px;
  border-bottom-left-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child {
  border-bottom-left-radius: 1px;
  border-bottom-right-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child td:first-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child td:first-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child td:first-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child td:first-child,
.panel > .table:last-child > tbody:last-child > tr:last-child th:first-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child th:first-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child th:first-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child th:first-child {
  border-bottom-left-radius: 1px;
}
.panel > .table:last-child > tbody:last-child > tr:last-child td:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child td:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child td:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child td:last-child,
.panel > .table:last-child > tbody:last-child > tr:last-child th:last-child,
.panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child th:last-child,
.panel > .table:last-child > tfoot:last-child > tr:last-child th:last-child,
.panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child th:last-child {
  border-bottom-right-radius: 1px;
}
.panel > .panel-body + .table,
.panel > .panel-body + .table-responsive,
.panel > .table + .panel-body,
.panel > .table-responsive + .panel-body {
  border-top: 1px solid #ddd;
}
.panel > .table > tbody:first-child > tr:first-child th,
.panel > .table > tbody:first-child > tr:first-child td {
  border-top: 0;
}
.panel > .table-bordered,
.panel > .table-responsive > .table-bordered {
  border: 0;
}
.panel > .table-bordered > thead > tr > th:first-child,
.panel > .table-responsive > .table-bordered > thead > tr > th:first-child,
.panel > .table-bordered > tbody > tr > th:first-child,
.panel > .table-responsive > .table-bordered > tbody > tr > th:first-child,
.panel > .table-bordered > tfoot > tr > th:first-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > th:first-child,
.panel > .table-bordered > thead > tr > td:first-child,
.panel > .table-responsive > .table-bordered > thead > tr > td:first-child,
.panel > .table-bordered > tbody > tr > td:first-child,
.panel > .table-responsive > .table-bordered > tbody > tr > td:first-child,
.panel > .table-bordered > tfoot > tr > td:first-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > td:first-child {
  border-left: 0;
}
.panel > .table-bordered > thead > tr > th:last-child,
.panel > .table-responsive > .table-bordered > thead > tr > th:last-child,
.panel > .table-bordered > tbody > tr > th:last-child,
.panel > .table-responsive > .table-bordered > tbody > tr > th:last-child,
.panel > .table-bordered > tfoot > tr > th:last-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > th:last-child,
.panel > .table-bordered > thead > tr > td:last-child,
.panel > .table-responsive > .table-bordered > thead > tr > td:last-child,
.panel > .table-bordered > tbody > tr > td:last-child,
.panel > .table-responsive > .table-bordered > tbody > tr > td:last-child,
.panel > .table-bordered > tfoot > tr > td:last-child,
.panel > .table-responsive > .table-bordered > tfoot > tr > td:last-child {
  border-right: 0;
}
.panel > .table-bordered > thead > tr:first-child > td,
.panel > .table-responsive > .table-bordered > thead > tr:first-child > td,
.panel > .table-bordered > tbody > tr:first-child > td,
.panel > .table-responsive > .table-bordered > tbody > tr:first-child > td,
.panel > .table-bordered > thead > tr:first-child > th,
.panel > .table-responsive > .table-bordered > thead > tr:first-child > th,
.panel > .table-bordered > tbody > tr:first-child > th,
.panel > .table-responsive > .table-bordered > tbody > tr:first-child > th {
  border-bottom: 0;
}
.panel > .table-bordered > tbody > tr:last-child > td,
.panel > .table-responsive > .table-bordered > tbody > tr:last-child > td,
.panel > .table-bordered > tfoot > tr:last-child > td,
.panel > .table-responsive > .table-bordered > tfoot > tr:last-child > td,
.panel > .table-bordered > tbody > tr:last-child > th,
.panel > .table-responsive > .table-bordered > tbody > tr:last-child > th,
.panel > .table-bordered > tfoot > tr:last-child > th,
.panel > .table-responsive > .table-bordered > tfoot > tr:last-child > th {
  border-bottom: 0;
}
.panel > .table-responsive {
  border: 0;
  margin-bottom: 0;
}
.panel-group {
  margin-bottom: 18px;
}
.panel-group .panel {
  margin-bottom: 0;
  border-radius: 2px;
}
.panel-group .panel + .panel {
  margin-top: 5px;
}
.panel-group .panel-heading {
  border-bottom: 0;
}
.panel-group .panel-heading + .panel-collapse > .panel-body,
.panel-group .panel-heading + .panel-collapse > .list-group {
  border-top: 1px solid #ddd;
}
.panel-group .panel-footer {
  border-top: 0;
}
.panel-group .panel-footer + .panel-collapse .panel-body {
  border-bottom: 1px solid #ddd;
}
.panel-default {
  border-color: #ddd;
}
.panel-default > .panel-heading {
  color: #333333;
  background-color: #f5f5f5;
  border-color: #ddd;
}
.panel-default > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #ddd;
}
.panel-default > .panel-heading .badge {
  color: #f5f5f5;
  background-color: #333333;
}
.panel-default > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #ddd;
}
.panel-primary {
  border-color: #337ab7;
}
.panel-primary > .panel-heading {
  color: #fff;
  background-color: #337ab7;
  border-color: #337ab7;
}
.panel-primary > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #337ab7;
}
.panel-primary > .panel-heading .badge {
  color: #337ab7;
  background-color: #fff;
}
.panel-primary > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #337ab7;
}
.panel-success {
  border-color: #d6e9c6;
}
.panel-success > .panel-heading {
  color: #3c763d;
  background-color: #dff0d8;
  border-color: #d6e9c6;
}
.panel-success > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #d6e9c6;
}
.panel-success > .panel-heading .badge {
  color: #dff0d8;
  background-color: #3c763d;
}
.panel-success > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #d6e9c6;
}
.panel-info {
  border-color: #bce8f1;
}
.panel-info > .panel-heading {
  color: #31708f;
  background-color: #d9edf7;
  border-color: #bce8f1;
}
.panel-info > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #bce8f1;
}
.panel-info > .panel-heading .badge {
  color: #d9edf7;
  background-color: #31708f;
}
.panel-info > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #bce8f1;
}
.panel-warning {
  border-color: #faebcc;
}
.panel-warning > .panel-heading {
  color: #8a6d3b;
  background-color: #fcf8e3;
  border-color: #faebcc;
}
.panel-warning > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #faebcc;
}
.panel-warning > .panel-heading .badge {
  color: #fcf8e3;
  background-color: #8a6d3b;
}
.panel-warning > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #faebcc;
}
.panel-danger {
  border-color: #ebccd1;
}
.panel-danger > .panel-heading {
  color: #a94442;
  background-color: #f2dede;
  border-color: #ebccd1;
}
.panel-danger > .panel-heading + .panel-collapse > .panel-body {
  border-top-color: #ebccd1;
}
.panel-danger > .panel-heading .badge {
  color: #f2dede;
  background-color: #a94442;
}
.panel-danger > .panel-footer + .panel-collapse > .panel-body {
  border-bottom-color: #ebccd1;
}
.embed-responsive {
  position: relative;
  display: block;
  height: 0;
  padding: 0;
  overflow: hidden;
}
.embed-responsive .embed-responsive-item,
.embed-responsive iframe,
.embed-responsive embed,
.embed-responsive object,
.embed-responsive video {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  height: 100%;
  width: 100%;
  border: 0;
}
.embed-responsive-16by9 {
  padding-bottom: 56.25%;
}
.embed-responsive-4by3 {
  padding-bottom: 75%;
}
.well {
  min-height: 20px;
  padding: 19px;
  margin-bottom: 20px;
  background-color: #f5f5f5;
  border: 1px solid #e3e3e3;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.05);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.05);
}
.well blockquote {
  border-color: #ddd;
  border-color: rgba(0, 0, 0, 0.15);
}
.well-lg {
  padding: 24px;
  border-radius: 3px;
}
.well-sm {
  padding: 9px;
  border-radius: 1px;
}
.close {
  float: right;
  font-size: 19.5px;
  font-weight: bold;
  line-height: 1;
  color: #000;
  text-shadow: 0 1px 0 #fff;
  opacity: 0.2;
  filter: alpha(opacity=20);
}
.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
  opacity: 0.5;
  filter: alpha(opacity=50);
}
button.close {
  padding: 0;
  cursor: pointer;
  background: transparent;
  border: 0;
  -webkit-appearance: none;
}
.modal-open {
  overflow: hidden;
}
.modal {
  display: none;
  overflow: hidden;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1050;
  -webkit-overflow-scrolling: touch;
  outline: 0;
}
.modal.fade .modal-dialog {
  -webkit-transform: translate(0, -25%);
  -ms-transform: translate(0, -25%);
  -o-transform: translate(0, -25%);
  transform: translate(0, -25%);
  -webkit-transition: -webkit-transform 0.3s ease-out;
  -moz-transition: -moz-transform 0.3s ease-out;
  -o-transition: -o-transform 0.3s ease-out;
  transition: transform 0.3s ease-out;
}
.modal.in .modal-dialog {
  -webkit-transform: translate(0, 0);
  -ms-transform: translate(0, 0);
  -o-transform: translate(0, 0);
  transform: translate(0, 0);
}
.modal-open .modal {
  overflow-x: hidden;
  overflow-y: auto;
}
.modal-dialog {
  position: relative;
  width: auto;
  margin: 10px;
}
.modal-content {
  position: relative;
  background-color: #fff;
  border: 1px solid #999;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 3px;
  -webkit-box-shadow: 0 3px 9px rgba(0, 0, 0, 0.5);
  box-shadow: 0 3px 9px rgba(0, 0, 0, 0.5);
  background-clip: padding-box;
  outline: 0;
}
.modal-backdrop {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1040;
  background-color: #000;
}
.modal-backdrop.fade {
  opacity: 0;
  filter: alpha(opacity=0);
}
.modal-backdrop.in {
  opacity: 0.5;
  filter: alpha(opacity=50);
}
.modal-header {
  padding: 15px;
  border-bottom: 1px solid #e5e5e5;
}
.modal-header .close {
  margin-top: -2px;
}
.modal-title {
  margin: 0;
  line-height: 1.42857143;
}
.modal-body {
  position: relative;
  padding: 15px;
}
.modal-footer {
  padding: 15px;
  text-align: right;
  border-top: 1px solid #e5e5e5;
}
.modal-footer .btn + .btn {
  margin-left: 5px;
  margin-bottom: 0;
}
.modal-footer .btn-group .btn + .btn {
  margin-left: -1px;
}
.modal-footer .btn-block + .btn-block {
  margin-left: 0;
}
.modal-scrollbar-measure {
  position: absolute;
  top: -9999px;
  width: 50px;
  height: 50px;
  overflow: scroll;
}
@media (min-width: 768px) {
  .modal-dialog {
    width: 600px;
    margin: 30px auto;
  }
  .modal-content {
    -webkit-box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
  }
  .modal-sm {
    width: 300px;
  }
}
@media (min-width: 992px) {
  .modal-lg {
    width: 900px;
  }
}
.tooltip {
  position: absolute;
  z-index: 1070;
  display: block;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-style: normal;
  font-weight: normal;
  letter-spacing: normal;
  line-break: auto;
  line-height: 1.42857143;
  text-align: left;
  text-align: start;
  text-decoration: none;
  text-shadow: none;
  text-transform: none;
  white-space: normal;
  word-break: normal;
  word-spacing: normal;
  word-wrap: normal;
  font-size: 12px;
  opacity: 0;
  filter: alpha(opacity=0);
}
.tooltip.in {
  opacity: 0.9;
  filter: alpha(opacity=90);
}
.tooltip.top {
  margin-top: -3px;
  padding: 5px 0;
}
.tooltip.right {
  margin-left: 3px;
  padding: 0 5px;
}
.tooltip.bottom {
  margin-top: 3px;
  padding: 5px 0;
}
.tooltip.left {
  margin-left: -3px;
  padding: 0 5px;
}
.tooltip-inner {
  max-width: 200px;
  padding: 3px 8px;
  color: #fff;
  text-align: center;
  background-color: #000;
  border-radius: 2px;
}
.tooltip-arrow {
  position: absolute;
  width: 0;
  height: 0;
  border-color: transparent;
  border-style: solid;
}
.tooltip.top .tooltip-arrow {
  bottom: 0;
  left: 50%;
  margin-left: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.top-left .tooltip-arrow {
  bottom: 0;
  right: 5px;
  margin-bottom: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.top-right .tooltip-arrow {
  bottom: 0;
  left: 5px;
  margin-bottom: -5px;
  border-width: 5px 5px 0;
  border-top-color: #000;
}
.tooltip.right .tooltip-arrow {
  top: 50%;
  left: 0;
  margin-top: -5px;
  border-width: 5px 5px 5px 0;
  border-right-color: #000;
}
.tooltip.left .tooltip-arrow {
  top: 50%;
  right: 0;
  margin-top: -5px;
  border-width: 5px 0 5px 5px;
  border-left-color: #000;
}
.tooltip.bottom .tooltip-arrow {
  top: 0;
  left: 50%;
  margin-left: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.tooltip.bottom-left .tooltip-arrow {
  top: 0;
  right: 5px;
  margin-top: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.tooltip.bottom-right .tooltip-arrow {
  top: 0;
  left: 5px;
  margin-top: -5px;
  border-width: 0 5px 5px;
  border-bottom-color: #000;
}
.popover {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1060;
  display: none;
  max-width: 276px;
  padding: 1px;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-style: normal;
  font-weight: normal;
  letter-spacing: normal;
  line-break: auto;
  line-height: 1.42857143;
  text-align: left;
  text-align: start;
  text-decoration: none;
  text-shadow: none;
  text-transform: none;
  white-space: normal;
  word-break: normal;
  word-spacing: normal;
  word-wrap: normal;
  font-size: 13px;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ccc;
  border: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 3px;
  -webkit-box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}
.popover.top {
  margin-top: -10px;
}
.popover.right {
  margin-left: 10px;
}
.popover.bottom {
  margin-top: 10px;
}
.popover.left {
  margin-left: -10px;
}
.popover-title {
  margin: 0;
  padding: 8px 14px;
  font-size: 13px;
  background-color: #f7f7f7;
  border-bottom: 1px solid #ebebeb;
  border-radius: 2px 2px 0 0;
}
.popover-content {
  padding: 9px 14px;
}
.popover > .arrow,
.popover > .arrow:after {
  position: absolute;
  display: block;
  width: 0;
  height: 0;
  border-color: transparent;
  border-style: solid;
}
.popover > .arrow {
  border-width: 11px;
}
.popover > .arrow:after {
  border-width: 10px;
  content: "";
}
.popover.top > .arrow {
  left: 50%;
  margin-left: -11px;
  border-bottom-width: 0;
  border-top-color: #999999;
  border-top-color: rgba(0, 0, 0, 0.25);
  bottom: -11px;
}
.popover.top > .arrow:after {
  content: " ";
  bottom: 1px;
  margin-left: -10px;
  border-bottom-width: 0;
  border-top-color: #fff;
}
.popover.right > .arrow {
  top: 50%;
  left: -11px;
  margin-top: -11px;
  border-left-width: 0;
  border-right-color: #999999;
  border-right-color: rgba(0, 0, 0, 0.25);
}
.popover.right > .arrow:after {
  content: " ";
  left: 1px;
  bottom: -10px;
  border-left-width: 0;
  border-right-color: #fff;
}
.popover.bottom > .arrow {
  left: 50%;
  margin-left: -11px;
  border-top-width: 0;
  border-bottom-color: #999999;
  border-bottom-color: rgba(0, 0, 0, 0.25);
  top: -11px;
}
.popover.bottom > .arrow:after {
  content: " ";
  top: 1px;
  margin-left: -10px;
  border-top-width: 0;
  border-bottom-color: #fff;
}
.popover.left > .arrow {
  top: 50%;
  right: -11px;
  margin-top: -11px;
  border-right-width: 0;
  border-left-color: #999999;
  border-left-color: rgba(0, 0, 0, 0.25);
}
.popover.left > .arrow:after {
  content: " ";
  right: 1px;
  border-right-width: 0;
  border-left-color: #fff;
  bottom: -10px;
}
.carousel {
  position: relative;
}
.carousel-inner {
  position: relative;
  overflow: hidden;
  width: 100%;
}
.carousel-inner > .item {
  display: none;
  position: relative;
  -webkit-transition: 0.6s ease-in-out left;
  -o-transition: 0.6s ease-in-out left;
  transition: 0.6s ease-in-out left;
}
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  line-height: 1;
}
@media all and (transform-3d), (-webkit-transform-3d) {
  .carousel-inner > .item {
    -webkit-transition: -webkit-transform 0.6s ease-in-out;
    -moz-transition: -moz-transform 0.6s ease-in-out;
    -o-transition: -o-transform 0.6s ease-in-out;
    transition: transform 0.6s ease-in-out;
    -webkit-backface-visibility: hidden;
    -moz-backface-visibility: hidden;
    backface-visibility: hidden;
    -webkit-perspective: 1000px;
    -moz-perspective: 1000px;
    perspective: 1000px;
  }
  .carousel-inner > .item.next,
  .carousel-inner > .item.active.right {
    -webkit-transform: translate3d(100%, 0, 0);
    transform: translate3d(100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.prev,
  .carousel-inner > .item.active.left {
    -webkit-transform: translate3d(-100%, 0, 0);
    transform: translate3d(-100%, 0, 0);
    left: 0;
  }
  .carousel-inner > .item.next.left,
  .carousel-inner > .item.prev.right,
  .carousel-inner > .item.active {
    -webkit-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
    left: 0;
  }
}
.carousel-inner > .active,
.carousel-inner > .next,
.carousel-inner > .prev {
  display: block;
}
.carousel-inner > .active {
  left: 0;
}
.carousel-inner > .next,
.carousel-inner > .prev {
  position: absolute;
  top: 0;
  width: 100%;
}
.carousel-inner > .next {
  left: 100%;
}
.carousel-inner > .prev {
  left: -100%;
}
.carousel-inner > .next.left,
.carousel-inner > .prev.right {
  left: 0;
}
.carousel-inner > .active.left {
  left: -100%;
}
.carousel-inner > .active.right {
  left: 100%;
}
.carousel-control {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  width: 15%;
  opacity: 0.5;
  filter: alpha(opacity=50);
  font-size: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
  background-color: rgba(0, 0, 0, 0);
}
.carousel-control.left {
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0.0001) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#80000000', endColorstr='#00000000', GradientType=1);
}
.carousel-control.right {
  left: auto;
  right: 0;
  background-image: -webkit-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: -o-linear-gradient(left, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-image: linear-gradient(to right, rgba(0, 0, 0, 0.0001) 0%, rgba(0, 0, 0, 0.5) 100%);
  background-repeat: repeat-x;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#00000000', endColorstr='#80000000', GradientType=1);
}
.carousel-control:hover,
.carousel-control:focus {
  outline: 0;
  color: #fff;
  text-decoration: none;
  opacity: 0.9;
  filter: alpha(opacity=90);
}
.carousel-control .icon-prev,
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-left,
.carousel-control .glyphicon-chevron-right {
  position: absolute;
  top: 50%;
  margin-top: -10px;
  z-index: 5;
  display: inline-block;
}
.carousel-control .icon-prev,
.carousel-control .glyphicon-chevron-left {
  left: 50%;
  margin-left: -10px;
}
.carousel-control .icon-next,
.carousel-control .glyphicon-chevron-right {
  right: 50%;
  margin-right: -10px;
}
.carousel-control .icon-prev,
.carousel-control .icon-next {
  width: 20px;
  height: 20px;
  line-height: 1;
  font-family: serif;
}
.carousel-control .icon-prev:before {
  content: '\2039';
}
.carousel-control .icon-next:before {
  content: '\203a';
}
.carousel-indicators {
  position: absolute;
  bottom: 10px;
  left: 50%;
  z-index: 15;
  width: 60%;
  margin-left: -30%;
  padding-left: 0;
  list-style: none;
  text-align: center;
}
.carousel-indicators li {
  display: inline-block;
  width: 10px;
  height: 10px;
  margin: 1px;
  text-indent: -999px;
  border: 1px solid #fff;
  border-radius: 10px;
  cursor: pointer;
  background-color: #000 \9;
  background-color: rgba(0, 0, 0, 0);
}
.carousel-indicators .active {
  margin: 0;
  width: 12px;
  height: 12px;
  background-color: #fff;
}
.carousel-caption {
  position: absolute;
  left: 15%;
  right: 15%;
  bottom: 20px;
  z-index: 10;
  padding-top: 20px;
  padding-bottom: 20px;
  color: #fff;
  text-align: center;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
}
.carousel-caption .btn {
  text-shadow: none;
}
@media screen and (min-width: 768px) {
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-prev,
  .carousel-control .icon-next {
    width: 30px;
    height: 30px;
    margin-top: -10px;
    font-size: 30px;
  }
  .carousel-control .glyphicon-chevron-left,
  .carousel-control .icon-prev {
    margin-left: -10px;
  }
  .carousel-control .glyphicon-chevron-right,
  .carousel-control .icon-next {
    margin-right: -10px;
  }
  .carousel-caption {
    left: 20%;
    right: 20%;
    padding-bottom: 30px;
  }
  .carousel-indicators {
    bottom: 20px;
  }
}
.clearfix:before,
.clearfix:after,
.dl-horizontal dd:before,
.dl-horizontal dd:after,
.container:before,
.container:after,
.container-fluid:before,
.container-fluid:after,
.row:before,
.row:after,
.form-horizontal .form-group:before,
.form-horizontal .form-group:after,
.btn-toolbar:before,
.btn-toolbar:after,
.btn-group-vertical > .btn-group:before,
.btn-group-vertical > .btn-group:after,
.nav:before,
.nav:after,
.navbar:before,
.navbar:after,
.navbar-header:before,
.navbar-header:after,
.navbar-collapse:before,
.navbar-collapse:after,
.pager:before,
.pager:after,
.panel-body:before,
.panel-body:after,
.modal-header:before,
.modal-header:after,
.modal-footer:before,
.modal-footer:after,
.item_buttons:before,
.item_buttons:after {
  content: " ";
  display: table;
}
.clearfix:after,
.dl-horizontal dd:after,
.container:after,
.container-fluid:after,
.row:after,
.form-horizontal .form-group:after,
.btn-toolbar:after,
.btn-group-vertical > .btn-group:after,
.nav:after,
.navbar:after,
.navbar-header:after,
.navbar-collapse:after,
.pager:after,
.panel-body:after,
.modal-header:after,
.modal-footer:after,
.item_buttons:after {
  clear: both;
}
.center-block {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.pull-right {
  float: right !important;
}
.pull-left {
  float: left !important;
}
.hide {
  display: none !important;
}
.show {
  display: block !important;
}
.invisible {
  visibility: hidden;
}
.text-hide {
  font: 0/0 a;
  color: transparent;
  text-shadow: none;
  background-color: transparent;
  border: 0;
}
.hidden {
  display: none !important;
}
.affix {
  position: fixed;
}
@-ms-viewport {
  width: device-width;
}
.visible-xs,
.visible-sm,
.visible-md,
.visible-lg {
  display: none !important;
}
.visible-xs-block,
.visible-xs-inline,
.visible-xs-inline-block,
.visible-sm-block,
.visible-sm-inline,
.visible-sm-inline-block,
.visible-md-block,
.visible-md-inline,
.visible-md-inline-block,
.visible-lg-block,
.visible-lg-inline,
.visible-lg-inline-block {
  display: none !important;
}
@media (max-width: 767px) {
  .visible-xs {
    display: block !important;
  }
  table.visible-xs {
    display: table !important;
  }
  tr.visible-xs {
    display: table-row !important;
  }
  th.visible-xs,
  td.visible-xs {
    display: table-cell !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-block {
    display: block !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-inline {
    display: inline !important;
  }
}
@media (max-width: 767px) {
  .visible-xs-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm {
    display: block !important;
  }
  table.visible-sm {
    display: table !important;
  }
  tr.visible-sm {
    display: table-row !important;
  }
  th.visible-sm,
  td.visible-sm {
    display: table-cell !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-block {
    display: block !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-inline {
    display: inline !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .visible-sm-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md {
    display: block !important;
  }
  table.visible-md {
    display: table !important;
  }
  tr.visible-md {
    display: table-row !important;
  }
  th.visible-md,
  td.visible-md {
    display: table-cell !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-block {
    display: block !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-inline {
    display: inline !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .visible-md-inline-block {
    display: inline-block !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg {
    display: block !important;
  }
  table.visible-lg {
    display: table !important;
  }
  tr.visible-lg {
    display: table-row !important;
  }
  th.visible-lg,
  td.visible-lg {
    display: table-cell !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-block {
    display: block !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-inline {
    display: inline !important;
  }
}
@media (min-width: 1200px) {
  .visible-lg-inline-block {
    display: inline-block !important;
  }
}
@media (max-width: 767px) {
  .hidden-xs {
    display: none !important;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  .hidden-sm {
    display: none !important;
  }
}
@media (min-width: 992px) and (max-width: 1199px) {
  .hidden-md {
    display: none !important;
  }
}
@media (min-width: 1200px) {
  .hidden-lg {
    display: none !important;
  }
}
.visible-print {
  display: none !important;
}
@media print {
  .visible-print {
    display: block !important;
  }
  table.visible-print {
    display: table !important;
  }
  tr.visible-print {
    display: table-row !important;
  }
  th.visible-print,
  td.visible-print {
    display: table-cell !important;
  }
}
.visible-print-block {
  display: none !important;
}
@media print {
  .visible-print-block {
    display: block !important;
  }
}
.visible-print-inline {
  display: none !important;
}
@media print {
  .visible-print-inline {
    display: inline !important;
  }
}
.visible-print-inline-block {
  display: none !important;
}
@media print {
  .visible-print-inline-block {
    display: inline-block !important;
  }
}
@media print {
  .hidden-print {
    display: none !important;
  }
}
/*!
*
* Font Awesome
*
*/
/*!
 *  Font Awesome 4.7.0 by @davegandy - http://fontawesome.io - @fontawesome
 *  License - http://fontawesome.io/license (Font: SIL OFL 1.1, CSS: MIT License)
 */
/* FONT PATH
 * -------------------------- */
@font-face {
  font-family: 'FontAwesome';
  src: url('../components/font-awesome/fonts/fontawesome-webfont.eot?v=4.7.0');
  src: url('../components/font-awesome/fonts/fontawesome-webfont.eot?#iefix&v=4.7.0') format('embedded-opentype'), url('../components/font-awesome/fonts/fontawesome-webfont.woff2?v=4.7.0') format('woff2'), url('../components/font-awesome/fonts/fontawesome-webfont.woff?v=4.7.0') format('woff'), url('../components/font-awesome/fonts/fontawesome-webfont.ttf?v=4.7.0') format('truetype'), url('../components/font-awesome/fonts/fontawesome-webfont.svg?v=4.7.0#fontawesomeregular') format('svg');
  font-weight: normal;
  font-style: normal;
}
.fa {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
/* makes the font 33% larger relative to the icon container */
.fa-lg {
  font-size: 1.33333333em;
  line-height: 0.75em;
  vertical-align: -15%;
}
.fa-2x {
  font-size: 2em;
}
.fa-3x {
  font-size: 3em;
}
.fa-4x {
  font-size: 4em;
}
.fa-5x {
  font-size: 5em;
}
.fa-fw {
  width: 1.28571429em;
  text-align: center;
}
.fa-ul {
  padding-left: 0;
  margin-left: 2.14285714em;
  list-style-type: none;
}
.fa-ul > li {
  position: relative;
}
.fa-li {
  position: absolute;
  left: -2.14285714em;
  width: 2.14285714em;
  top: 0.14285714em;
  text-align: center;
}
.fa-li.fa-lg {
  left: -1.85714286em;
}
.fa-border {
  padding: .2em .25em .15em;
  border: solid 0.08em #eee;
  border-radius: .1em;
}
.fa-pull-left {
  float: left;
}
.fa-pull-right {
  float: right;
}
.fa.fa-pull-left {
  margin-right: .3em;
}
.fa.fa-pull-right {
  margin-left: .3em;
}
/* Deprecated as of 4.4.0 */
.pull-right {
  float: right;
}
.pull-left {
  float: left;
}
.fa.pull-left {
  margin-right: .3em;
}
.fa.pull-right {
  margin-left: .3em;
}
.fa-spin {
  -webkit-animation: fa-spin 2s infinite linear;
  animation: fa-spin 2s infinite linear;
}
.fa-pulse {
  -webkit-animation: fa-spin 1s infinite steps(8);
  animation: fa-spin 1s infinite steps(8);
}
@-webkit-keyframes fa-spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(359deg);
    transform: rotate(359deg);
  }
}
@keyframes fa-spin {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(359deg);
    transform: rotate(359deg);
  }
}
.fa-rotate-90 {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=1)";
  -webkit-transform: rotate(90deg);
  -ms-transform: rotate(90deg);
  transform: rotate(90deg);
}
.fa-rotate-180 {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=2)";
  -webkit-transform: rotate(180deg);
  -ms-transform: rotate(180deg);
  transform: rotate(180deg);
}
.fa-rotate-270 {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=3)";
  -webkit-transform: rotate(270deg);
  -ms-transform: rotate(270deg);
  transform: rotate(270deg);
}
.fa-flip-horizontal {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=0, mirror=1)";
  -webkit-transform: scale(-1, 1);
  -ms-transform: scale(-1, 1);
  transform: scale(-1, 1);
}
.fa-flip-vertical {
  -ms-filter: "progid:DXImageTransform.Microsoft.BasicImage(rotation=2, mirror=1)";
  -webkit-transform: scale(1, -1);
  -ms-transform: scale(1, -1);
  transform: scale(1, -1);
}
:root .fa-rotate-90,
:root .fa-rotate-180,
:root .fa-rotate-270,
:root .fa-flip-horizontal,
:root .fa-flip-vertical {
  filter: none;
}
.fa-stack {
  position: relative;
  display: inline-block;
  width: 2em;
  height: 2em;
  line-height: 2em;
  vertical-align: middle;
}
.fa-stack-1x,
.fa-stack-2x {
  position: absolute;
  left: 0;
  width: 100%;
  text-align: center;
}
.fa-stack-1x {
  line-height: inherit;
}
.fa-stack-2x {
  font-size: 2em;
}
.fa-inverse {
  color: #fff;
}
/* Font Awesome uses the Unicode Private Use Area (PUA) to ensure screen
   readers do not read off random characters that represent icons */
.fa-glass:before {
  content: "\f000";
}
.fa-music:before {
  content: "\f001";
}
.fa-search:before {
  content: "\f002";
}
.fa-envelope-o:before {
  content: "\f003";
}
.fa-heart:before {
  content: "\f004";
}
.fa-star:before {
  content: "\f005";
}
.fa-star-o:before {
  content: "\f006";
}
.fa-user:before {
  content: "\f007";
}
.fa-film:before {
  content: "\f008";
}
.fa-th-large:before {
  content: "\f009";
}
.fa-th:before {
  content: "\f00a";
}
.fa-th-list:before {
  content: "\f00b";
}
.fa-check:before {
  content: "\f00c";
}
.fa-remove:before,
.fa-close:before,
.fa-times:before {
  content: "\f00d";
}
.fa-search-plus:before {
  content: "\f00e";
}
.fa-search-minus:before {
  content: "\f010";
}
.fa-power-off:before {
  content: "\f011";
}
.fa-signal:before {
  content: "\f012";
}
.fa-gear:before,
.fa-cog:before {
  content: "\f013";
}
.fa-trash-o:before {
  content: "\f014";
}
.fa-home:before {
  content: "\f015";
}
.fa-file-o:before {
  content: "\f016";
}
.fa-clock-o:before {
  content: "\f017";
}
.fa-road:before {
  content: "\f018";
}
.fa-download:before {
  content: "\f019";
}
.fa-arrow-circle-o-down:before {
  content: "\f01a";
}
.fa-arrow-circle-o-up:before {
  content: "\f01b";
}
.fa-inbox:before {
  content: "\f01c";
}
.fa-play-circle-o:before {
  content: "\f01d";
}
.fa-rotate-right:before,
.fa-repeat:before {
  content: "\f01e";
}
.fa-refresh:before {
  content: "\f021";
}
.fa-list-alt:before {
  content: "\f022";
}
.fa-lock:before {
  content: "\f023";
}
.fa-flag:before {
  content: "\f024";
}
.fa-headphones:before {
  content: "\f025";
}
.fa-volume-off:before {
  content: "\f026";
}
.fa-volume-down:before {
  content: "\f027";
}
.fa-volume-up:before {
  content: "\f028";
}
.fa-qrcode:before {
  content: "\f029";
}
.fa-barcode:before {
  content: "\f02a";
}
.fa-tag:before {
  content: "\f02b";
}
.fa-tags:before {
  content: "\f02c";
}
.fa-book:before {
  content: "\f02d";
}
.fa-bookmark:before {
  content: "\f02e";
}
.fa-print:before {
  content: "\f02f";
}
.fa-camera:before {
  content: "\f030";
}
.fa-font:before {
  content: "\f031";
}
.fa-bold:before {
  content: "\f032";
}
.fa-italic:before {
  content: "\f033";
}
.fa-text-height:before {
  content: "\f034";
}
.fa-text-width:before {
  content: "\f035";
}
.fa-align-left:before {
  content: "\f036";
}
.fa-align-center:before {
  content: "\f037";
}
.fa-align-right:before {
  content: "\f038";
}
.fa-align-justify:before {
  content: "\f039";
}
.fa-list:before {
  content: "\f03a";
}
.fa-dedent:before,
.fa-outdent:before {
  content: "\f03b";
}
.fa-indent:before {
  content: "\f03c";
}
.fa-video-camera:before {
  content: "\f03d";
}
.fa-photo:before,
.fa-image:before,
.fa-picture-o:before {
  content: "\f03e";
}
.fa-pencil:before {
  content: "\f040";
}
.fa-map-marker:before {
  content: "\f041";
}
.fa-adjust:before {
  content: "\f042";
}
.fa-tint:before {
  content: "\f043";
}
.fa-edit:before,
.fa-pencil-square-o:before {
  content: "\f044";
}
.fa-share-square-o:before {
  content: "\f045";
}
.fa-check-square-o:before {
  content: "\f046";
}
.fa-arrows:before {
  content: "\f047";
}
.fa-step-backward:before {
  content: "\f048";
}
.fa-fast-backward:before {
  content: "\f049";
}
.fa-backward:before {
  content: "\f04a";
}
.fa-play:before {
  content: "\f04b";
}
.fa-pause:before {
  content: "\f04c";
}
.fa-stop:before {
  content: "\f04d";
}
.fa-forward:before {
  content: "\f04e";
}
.fa-fast-forward:before {
  content: "\f050";
}
.fa-step-forward:before {
  content: "\f051";
}
.fa-eject:before {
  content: "\f052";
}
.fa-chevron-left:before {
  content: "\f053";
}
.fa-chevron-right:before {
  content: "\f054";
}
.fa-plus-circle:before {
  content: "\f055";
}
.fa-minus-circle:before {
  content: "\f056";
}
.fa-times-circle:before {
  content: "\f057";
}
.fa-check-circle:before {
  content: "\f058";
}
.fa-question-circle:before {
  content: "\f059";
}
.fa-info-circle:before {
  content: "\f05a";
}
.fa-crosshairs:before {
  content: "\f05b";
}
.fa-times-circle-o:before {
  content: "\f05c";
}
.fa-check-circle-o:before {
  content: "\f05d";
}
.fa-ban:before {
  content: "\f05e";
}
.fa-arrow-left:before {
  content: "\f060";
}
.fa-arrow-right:before {
  content: "\f061";
}
.fa-arrow-up:before {
  content: "\f062";
}
.fa-arrow-down:before {
  content: "\f063";
}
.fa-mail-forward:before,
.fa-share:before {
  content: "\f064";
}
.fa-expand:before {
  content: "\f065";
}
.fa-compress:before {
  content: "\f066";
}
.fa-plus:before {
  content: "\f067";
}
.fa-minus:before {
  content: "\f068";
}
.fa-asterisk:before {
  content: "\f069";
}
.fa-exclamation-circle:before {
  content: "\f06a";
}
.fa-gift:before {
  content: "\f06b";
}
.fa-leaf:before {
  content: "\f06c";
}
.fa-fire:before {
  content: "\f06d";
}
.fa-eye:before {
  content: "\f06e";
}
.fa-eye-slash:before {
  content: "\f070";
}
.fa-warning:before,
.fa-exclamation-triangle:before {
  content: "\f071";
}
.fa-plane:before {
  content: "\f072";
}
.fa-calendar:before {
  content: "\f073";
}
.fa-random:before {
  content: "\f074";
}
.fa-comment:before {
  content: "\f075";
}
.fa-magnet:before {
  content: "\f076";
}
.fa-chevron-up:before {
  content: "\f077";
}
.fa-chevron-down:before {
  content: "\f078";
}
.fa-retweet:before {
  content: "\f079";
}
.fa-shopping-cart:before {
  content: "\f07a";
}
.fa-folder:before {
  content: "\f07b";
}
.fa-folder-open:before {
  content: "\f07c";
}
.fa-arrows-v:before {
  content: "\f07d";
}
.fa-arrows-h:before {
  content: "\f07e";
}
.fa-bar-chart-o:before,
.fa-bar-chart:before {
  content: "\f080";
}
.fa-twitter-square:before {
  content: "\f081";
}
.fa-facebook-square:before {
  content: "\f082";
}
.fa-camera-retro:before {
  content: "\f083";
}
.fa-key:before {
  content: "\f084";
}
.fa-gears:before,
.fa-cogs:before {
  content: "\f085";
}
.fa-comments:before {
  content: "\f086";
}
.fa-thumbs-o-up:before {
  content: "\f087";
}
.fa-thumbs-o-down:before {
  content: "\f088";
}
.fa-star-half:before {
  content: "\f089";
}
.fa-heart-o:before {
  content: "\f08a";
}
.fa-sign-out:before {
  content: "\f08b";
}
.fa-linkedin-square:before {
  content: "\f08c";
}
.fa-thumb-tack:before {
  content: "\f08d";
}
.fa-external-link:before {
  content: "\f08e";
}
.fa-sign-in:before {
  content: "\f090";
}
.fa-trophy:before {
  content: "\f091";
}
.fa-github-square:before {
  content: "\f092";
}
.fa-upload:before {
  content: "\f093";
}
.fa-lemon-o:before {
  content: "\f094";
}
.fa-phone:before {
  content: "\f095";
}
.fa-square-o:before {
  content: "\f096";
}
.fa-bookmark-o:before {
  content: "\f097";
}
.fa-phone-square:before {
  content: "\f098";
}
.fa-twitter:before {
  content: "\f099";
}
.fa-facebook-f:before,
.fa-facebook:before {
  content: "\f09a";
}
.fa-github:before {
  content: "\f09b";
}
.fa-unlock:before {
  content: "\f09c";
}
.fa-credit-card:before {
  content: "\f09d";
}
.fa-feed:before,
.fa-rss:before {
  content: "\f09e";
}
.fa-hdd-o:before {
  content: "\f0a0";
}
.fa-bullhorn:before {
  content: "\f0a1";
}
.fa-bell:before {
  content: "\f0f3";
}
.fa-certificate:before {
  content: "\f0a3";
}
.fa-hand-o-right:before {
  content: "\f0a4";
}
.fa-hand-o-left:before {
  content: "\f0a5";
}
.fa-hand-o-up:before {
  content: "\f0a6";
}
.fa-hand-o-down:before {
  content: "\f0a7";
}
.fa-arrow-circle-left:before {
  content: "\f0a8";
}
.fa-arrow-circle-right:before {
  content: "\f0a9";
}
.fa-arrow-circle-up:before {
  content: "\f0aa";
}
.fa-arrow-circle-down:before {
  content: "\f0ab";
}
.fa-globe:before {
  content: "\f0ac";
}
.fa-wrench:before {
  content: "\f0ad";
}
.fa-tasks:before {
  content: "\f0ae";
}
.fa-filter:before {
  content: "\f0b0";
}
.fa-briefcase:before {
  content: "\f0b1";
}
.fa-arrows-alt:before {
  content: "\f0b2";
}
.fa-group:before,
.fa-users:before {
  content: "\f0c0";
}
.fa-chain:before,
.fa-link:before {
  content: "\f0c1";
}
.fa-cloud:before {
  content: "\f0c2";
}
.fa-flask:before {
  content: "\f0c3";
}
.fa-cut:before,
.fa-scissors:before {
  content: "\f0c4";
}
.fa-copy:before,
.fa-files-o:before {
  content: "\f0c5";
}
.fa-paperclip:before {
  content: "\f0c6";
}
.fa-save:before,
.fa-floppy-o:before {
  content: "\f0c7";
}
.fa-square:before {
  content: "\f0c8";
}
.fa-navicon:before,
.fa-reorder:before,
.fa-bars:before {
  content: "\f0c9";
}
.fa-list-ul:before {
  content: "\f0ca";
}
.fa-list-ol:before {
  content: "\f0cb";
}
.fa-strikethrough:before {
  content: "\f0cc";
}
.fa-underline:before {
  content: "\f0cd";
}
.fa-table:before {
  content: "\f0ce";
}
.fa-magic:before {
  content: "\f0d0";
}
.fa-truck:before {
  content: "\f0d1";
}
.fa-pinterest:before {
  content: "\f0d2";
}
.fa-pinterest-square:before {
  content: "\f0d3";
}
.fa-google-plus-square:before {
  content: "\f0d4";
}
.fa-google-plus:before {
  content: "\f0d5";
}
.fa-money:before {
  content: "\f0d6";
}
.fa-caret-down:before {
  content: "\f0d7";
}
.fa-caret-up:before {
  content: "\f0d8";
}
.fa-caret-left:before {
  content: "\f0d9";
}
.fa-caret-right:before {
  content: "\f0da";
}
.fa-columns:before {
  content: "\f0db";
}
.fa-unsorted:before,
.fa-sort:before {
  content: "\f0dc";
}
.fa-sort-down:before,
.fa-sort-desc:before {
  content: "\f0dd";
}
.fa-sort-up:before,
.fa-sort-asc:before {
  content: "\f0de";
}
.fa-envelope:before {
  content: "\f0e0";
}
.fa-linkedin:before {
  content: "\f0e1";
}
.fa-rotate-left:before,
.fa-undo:before {
  content: "\f0e2";
}
.fa-legal:before,
.fa-gavel:before {
  content: "\f0e3";
}
.fa-dashboard:before,
.fa-tachometer:before {
  content: "\f0e4";
}
.fa-comment-o:before {
  content: "\f0e5";
}
.fa-comments-o:before {
  content: "\f0e6";
}
.fa-flash:before,
.fa-bolt:before {
  content: "\f0e7";
}
.fa-sitemap:before {
  content: "\f0e8";
}
.fa-umbrella:before {
  content: "\f0e9";
}
.fa-paste:before,
.fa-clipboard:before {
  content: "\f0ea";
}
.fa-lightbulb-o:before {
  content: "\f0eb";
}
.fa-exchange:before {
  content: "\f0ec";
}
.fa-cloud-download:before {
  content: "\f0ed";
}
.fa-cloud-upload:before {
  content: "\f0ee";
}
.fa-user-md:before {
  content: "\f0f0";
}
.fa-stethoscope:before {
  content: "\f0f1";
}
.fa-suitcase:before {
  content: "\f0f2";
}
.fa-bell-o:before {
  content: "\f0a2";
}
.fa-coffee:before {
  content: "\f0f4";
}
.fa-cutlery:before {
  content: "\f0f5";
}
.fa-file-text-o:before {
  content: "\f0f6";
}
.fa-building-o:before {
  content: "\f0f7";
}
.fa-hospital-o:before {
  content: "\f0f8";
}
.fa-ambulance:before {
  content: "\f0f9";
}
.fa-medkit:before {
  content: "\f0fa";
}
.fa-fighter-jet:before {
  content: "\f0fb";
}
.fa-beer:before {
  content: "\f0fc";
}
.fa-h-square:before {
  content: "\f0fd";
}
.fa-plus-square:before {
  content: "\f0fe";
}
.fa-angle-double-left:before {
  content: "\f100";
}
.fa-angle-double-right:before {
  content: "\f101";
}
.fa-angle-double-up:before {
  content: "\f102";
}
.fa-angle-double-down:before {
  content: "\f103";
}
.fa-angle-left:before {
  content: "\f104";
}
.fa-angle-right:before {
  content: "\f105";
}
.fa-angle-up:before {
  content: "\f106";
}
.fa-angle-down:before {
  content: "\f107";
}
.fa-desktop:before {
  content: "\f108";
}
.fa-laptop:before {
  content: "\f109";
}
.fa-tablet:before {
  content: "\f10a";
}
.fa-mobile-phone:before,
.fa-mobile:before {
  content: "\f10b";
}
.fa-circle-o:before {
  content: "\f10c";
}
.fa-quote-left:before {
  content: "\f10d";
}
.fa-quote-right:before {
  content: "\f10e";
}
.fa-spinner:before {
  content: "\f110";
}
.fa-circle:before {
  content: "\f111";
}
.fa-mail-reply:before,
.fa-reply:before {
  content: "\f112";
}
.fa-github-alt:before {
  content: "\f113";
}
.fa-folder-o:before {
  content: "\f114";
}
.fa-folder-open-o:before {
  content: "\f115";
}
.fa-smile-o:before {
  content: "\f118";
}
.fa-frown-o:before {
  content: "\f119";
}
.fa-meh-o:before {
  content: "\f11a";
}
.fa-gamepad:before {
  content: "\f11b";
}
.fa-keyboard-o:before {
  content: "\f11c";
}
.fa-flag-o:before {
  content: "\f11d";
}
.fa-flag-checkered:before {
  content: "\f11e";
}
.fa-terminal:before {
  content: "\f120";
}
.fa-code:before {
  content: "\f121";
}
.fa-mail-reply-all:before,
.fa-reply-all:before {
  content: "\f122";
}
.fa-star-half-empty:before,
.fa-star-half-full:before,
.fa-star-half-o:before {
  content: "\f123";
}
.fa-location-arrow:before {
  content: "\f124";
}
.fa-crop:before {
  content: "\f125";
}
.fa-code-fork:before {
  content: "\f126";
}
.fa-unlink:before,
.fa-chain-broken:before {
  content: "\f127";
}
.fa-question:before {
  content: "\f128";
}
.fa-info:before {
  content: "\f129";
}
.fa-exclamation:before {
  content: "\f12a";
}
.fa-superscript:before {
  content: "\f12b";
}
.fa-subscript:before {
  content: "\f12c";
}
.fa-eraser:before {
  content: "\f12d";
}
.fa-puzzle-piece:before {
  content: "\f12e";
}
.fa-microphone:before {
  content: "\f130";
}
.fa-microphone-slash:before {
  content: "\f131";
}
.fa-shield:before {
  content: "\f132";
}
.fa-calendar-o:before {
  content: "\f133";
}
.fa-fire-extinguisher:before {
  content: "\f134";
}
.fa-rocket:before {
  content: "\f135";
}
.fa-maxcdn:before {
  content: "\f136";
}
.fa-chevron-circle-left:before {
  content: "\f137";
}
.fa-chevron-circle-right:before {
  content: "\f138";
}
.fa-chevron-circle-up:before {
  content: "\f139";
}
.fa-chevron-circle-down:before {
  content: "\f13a";
}
.fa-html5:before {
  content: "\f13b";
}
.fa-css3:before {
  content: "\f13c";
}
.fa-anchor:before {
  content: "\f13d";
}
.fa-unlock-alt:before {
  content: "\f13e";
}
.fa-bullseye:before {
  content: "\f140";
}
.fa-ellipsis-h:before {
  content: "\f141";
}
.fa-ellipsis-v:before {
  content: "\f142";
}
.fa-rss-square:before {
  content: "\f143";
}
.fa-play-circle:before {
  content: "\f144";
}
.fa-ticket:before {
  content: "\f145";
}
.fa-minus-square:before {
  content: "\f146";
}
.fa-minus-square-o:before {
  content: "\f147";
}
.fa-level-up:before {
  content: "\f148";
}
.fa-level-down:before {
  content: "\f149";
}
.fa-check-square:before {
  content: "\f14a";
}
.fa-pencil-square:before {
  content: "\f14b";
}
.fa-external-link-square:before {
  content: "\f14c";
}
.fa-share-square:before {
  content: "\f14d";
}
.fa-compass:before {
  content: "\f14e";
}
.fa-toggle-down:before,
.fa-caret-square-o-down:before {
  content: "\f150";
}
.fa-toggle-up:before,
.fa-caret-square-o-up:before {
  content: "\f151";
}
.fa-toggle-right:before,
.fa-caret-square-o-right:before {
  content: "\f152";
}
.fa-euro:before,
.fa-eur:before {
  content: "\f153";
}
.fa-gbp:before {
  content: "\f154";
}
.fa-dollar:before,
.fa-usd:before {
  content: "\f155";
}
.fa-rupee:before,
.fa-inr:before {
  content: "\f156";
}
.fa-cny:before,
.fa-rmb:before,
.fa-yen:before,
.fa-jpy:before {
  content: "\f157";
}
.fa-ruble:before,
.fa-rouble:before,
.fa-rub:before {
  content: "\f158";
}
.fa-won:before,
.fa-krw:before {
  content: "\f159";
}
.fa-bitcoin:before,
.fa-btc:before {
  content: "\f15a";
}
.fa-file:before {
  content: "\f15b";
}
.fa-file-text:before {
  content: "\f15c";
}
.fa-sort-alpha-asc:before {
  content: "\f15d";
}
.fa-sort-alpha-desc:before {
  content: "\f15e";
}
.fa-sort-amount-asc:before {
  content: "\f160";
}
.fa-sort-amount-desc:before {
  content: "\f161";
}
.fa-sort-numeric-asc:before {
  content: "\f162";
}
.fa-sort-numeric-desc:before {
  content: "\f163";
}
.fa-thumbs-up:before {
  content: "\f164";
}
.fa-thumbs-down:before {
  content: "\f165";
}
.fa-youtube-square:before {
  content: "\f166";
}
.fa-youtube:before {
  content: "\f167";
}
.fa-xing:before {
  content: "\f168";
}
.fa-xing-square:before {
  content: "\f169";
}
.fa-youtube-play:before {
  content: "\f16a";
}
.fa-dropbox:before {
  content: "\f16b";
}
.fa-stack-overflow:before {
  content: "\f16c";
}
.fa-instagram:before {
  content: "\f16d";
}
.fa-flickr:before {
  content: "\f16e";
}
.fa-adn:before {
  content: "\f170";
}
.fa-bitbucket:before {
  content: "\f171";
}
.fa-bitbucket-square:before {
  content: "\f172";
}
.fa-tumblr:before {
  content: "\f173";
}
.fa-tumblr-square:before {
  content: "\f174";
}
.fa-long-arrow-down:before {
  content: "\f175";
}
.fa-long-arrow-up:before {
  content: "\f176";
}
.fa-long-arrow-left:before {
  content: "\f177";
}
.fa-long-arrow-right:before {
  content: "\f178";
}
.fa-apple:before {
  content: "\f179";
}
.fa-windows:before {
  content: "\f17a";
}
.fa-android:before {
  content: "\f17b";
}
.fa-linux:before {
  content: "\f17c";
}
.fa-dribbble:before {
  content: "\f17d";
}
.fa-skype:before {
  content: "\f17e";
}
.fa-foursquare:before {
  content: "\f180";
}
.fa-trello:before {
  content: "\f181";
}
.fa-female:before {
  content: "\f182";
}
.fa-male:before {
  content: "\f183";
}
.fa-gittip:before,
.fa-gratipay:before {
  content: "\f184";
}
.fa-sun-o:before {
  content: "\f185";
}
.fa-moon-o:before {
  content: "\f186";
}
.fa-archive:before {
  content: "\f187";
}
.fa-bug:before {
  content: "\f188";
}
.fa-vk:before {
  content: "\f189";
}
.fa-weibo:before {
  content: "\f18a";
}
.fa-renren:before {
  content: "\f18b";
}
.fa-pagelines:before {
  content: "\f18c";
}
.fa-stack-exchange:before {
  content: "\f18d";
}
.fa-arrow-circle-o-right:before {
  content: "\f18e";
}
.fa-arrow-circle-o-left:before {
  content: "\f190";
}
.fa-toggle-left:before,
.fa-caret-square-o-left:before {
  content: "\f191";
}
.fa-dot-circle-o:before {
  content: "\f192";
}
.fa-wheelchair:before {
  content: "\f193";
}
.fa-vimeo-square:before {
  content: "\f194";
}
.fa-turkish-lira:before,
.fa-try:before {
  content: "\f195";
}
.fa-plus-square-o:before {
  content: "\f196";
}
.fa-space-shuttle:before {
  content: "\f197";
}
.fa-slack:before {
  content: "\f198";
}
.fa-envelope-square:before {
  content: "\f199";
}
.fa-wordpress:before {
  content: "\f19a";
}
.fa-openid:before {
  content: "\f19b";
}
.fa-institution:before,
.fa-bank:before,
.fa-university:before {
  content: "\f19c";
}
.fa-mortar-board:before,
.fa-graduation-cap:before {
  content: "\f19d";
}
.fa-yahoo:before {
  content: "\f19e";
}
.fa-google:before {
  content: "\f1a0";
}
.fa-reddit:before {
  content: "\f1a1";
}
.fa-reddit-square:before {
  content: "\f1a2";
}
.fa-stumbleupon-circle:before {
  content: "\f1a3";
}
.fa-stumbleupon:before {
  content: "\f1a4";
}
.fa-delicious:before {
  content: "\f1a5";
}
.fa-digg:before {
  content: "\f1a6";
}
.fa-pied-piper-pp:before {
  content: "\f1a7";
}
.fa-pied-piper-alt:before {
  content: "\f1a8";
}
.fa-drupal:before {
  content: "\f1a9";
}
.fa-joomla:before {
  content: "\f1aa";
}
.fa-language:before {
  content: "\f1ab";
}
.fa-fax:before {
  content: "\f1ac";
}
.fa-building:before {
  content: "\f1ad";
}
.fa-child:before {
  content: "\f1ae";
}
.fa-paw:before {
  content: "\f1b0";
}
.fa-spoon:before {
  content: "\f1b1";
}
.fa-cube:before {
  content: "\f1b2";
}
.fa-cubes:before {
  content: "\f1b3";
}
.fa-behance:before {
  content: "\f1b4";
}
.fa-behance-square:before {
  content: "\f1b5";
}
.fa-steam:before {
  content: "\f1b6";
}
.fa-steam-square:before {
  content: "\f1b7";
}
.fa-recycle:before {
  content: "\f1b8";
}
.fa-automobile:before,
.fa-car:before {
  content: "\f1b9";
}
.fa-cab:before,
.fa-taxi:before {
  content: "\f1ba";
}
.fa-tree:before {
  content: "\f1bb";
}
.fa-spotify:before {
  content: "\f1bc";
}
.fa-deviantart:before {
  content: "\f1bd";
}
.fa-soundcloud:before {
  content: "\f1be";
}
.fa-database:before {
  content: "\f1c0";
}
.fa-file-pdf-o:before {
  content: "\f1c1";
}
.fa-file-word-o:before {
  content: "\f1c2";
}
.fa-file-excel-o:before {
  content: "\f1c3";
}
.fa-file-powerpoint-o:before {
  content: "\f1c4";
}
.fa-file-photo-o:before,
.fa-file-picture-o:before,
.fa-file-image-o:before {
  content: "\f1c5";
}
.fa-file-zip-o:before,
.fa-file-archive-o:before {
  content: "\f1c6";
}
.fa-file-sound-o:before,
.fa-file-audio-o:before {
  content: "\f1c7";
}
.fa-file-movie-o:before,
.fa-file-video-o:before {
  content: "\f1c8";
}
.fa-file-code-o:before {
  content: "\f1c9";
}
.fa-vine:before {
  content: "\f1ca";
}
.fa-codepen:before {
  content: "\f1cb";
}
.fa-jsfiddle:before {
  content: "\f1cc";
}
.fa-life-bouy:before,
.fa-life-buoy:before,
.fa-life-saver:before,
.fa-support:before,
.fa-life-ring:before {
  content: "\f1cd";
}
.fa-circle-o-notch:before {
  content: "\f1ce";
}
.fa-ra:before,
.fa-resistance:before,
.fa-rebel:before {
  content: "\f1d0";
}
.fa-ge:before,
.fa-empire:before {
  content: "\f1d1";
}
.fa-git-square:before {
  content: "\f1d2";
}
.fa-git:before {
  content: "\f1d3";
}
.fa-y-combinator-square:before,
.fa-yc-square:before,
.fa-hacker-news:before {
  content: "\f1d4";
}
.fa-tencent-weibo:before {
  content: "\f1d5";
}
.fa-qq:before {
  content: "\f1d6";
}
.fa-wechat:before,
.fa-weixin:before {
  content: "\f1d7";
}
.fa-send:before,
.fa-paper-plane:before {
  content: "\f1d8";
}
.fa-send-o:before,
.fa-paper-plane-o:before {
  content: "\f1d9";
}
.fa-history:before {
  content: "\f1da";
}
.fa-circle-thin:before {
  content: "\f1db";
}
.fa-header:before {
  content: "\f1dc";
}
.fa-paragraph:before {
  content: "\f1dd";
}
.fa-sliders:before {
  content: "\f1de";
}
.fa-share-alt:before {
  content: "\f1e0";
}
.fa-share-alt-square:before {
  content: "\f1e1";
}
.fa-bomb:before {
  content: "\f1e2";
}
.fa-soccer-ball-o:before,
.fa-futbol-o:before {
  content: "\f1e3";
}
.fa-tty:before {
  content: "\f1e4";
}
.fa-binoculars:before {
  content: "\f1e5";
}
.fa-plug:before {
  content: "\f1e6";
}
.fa-slideshare:before {
  content: "\f1e7";
}
.fa-twitch:before {
  content: "\f1e8";
}
.fa-yelp:before {
  content: "\f1e9";
}
.fa-newspaper-o:before {
  content: "\f1ea";
}
.fa-wifi:before {
  content: "\f1eb";
}
.fa-calculator:before {
  content: "\f1ec";
}
.fa-paypal:before {
  content: "\f1ed";
}
.fa-google-wallet:before {
  content: "\f1ee";
}
.fa-cc-visa:before {
  content: "\f1f0";
}
.fa-cc-mastercard:before {
  content: "\f1f1";
}
.fa-cc-discover:before {
  content: "\f1f2";
}
.fa-cc-amex:before {
  content: "\f1f3";
}
.fa-cc-paypal:before {
  content: "\f1f4";
}
.fa-cc-stripe:before {
  content: "\f1f5";
}
.fa-bell-slash:before {
  content: "\f1f6";
}
.fa-bell-slash-o:before {
  content: "\f1f7";
}
.fa-trash:before {
  content: "\f1f8";
}
.fa-copyright:before {
  content: "\f1f9";
}
.fa-at:before {
  content: "\f1fa";
}
.fa-eyedropper:before {
  content: "\f1fb";
}
.fa-paint-brush:before {
  content: "\f1fc";
}
.fa-birthday-cake:before {
  content: "\f1fd";
}
.fa-area-chart:before {
  content: "\f1fe";
}
.fa-pie-chart:before {
  content: "\f200";
}
.fa-line-chart:before {
  content: "\f201";
}
.fa-lastfm:before {
  content: "\f202";
}
.fa-lastfm-square:before {
  content: "\f203";
}
.fa-toggle-off:before {
  content: "\f204";
}
.fa-toggle-on:before {
  content: "\f205";
}
.fa-bicycle:before {
  content: "\f206";
}
.fa-bus:before {
  content: "\f207";
}
.fa-ioxhost:before {
  content: "\f208";
}
.fa-angellist:before {
  content: "\f209";
}
.fa-cc:before {
  content: "\f20a";
}
.fa-shekel:before,
.fa-sheqel:before,
.fa-ils:before {
  content: "\f20b";
}
.fa-meanpath:before {
  content: "\f20c";
}
.fa-buysellads:before {
  content: "\f20d";
}
.fa-connectdevelop:before {
  content: "\f20e";
}
.fa-dashcube:before {
  content: "\f210";
}
.fa-forumbee:before {
  content: "\f211";
}
.fa-leanpub:before {
  content: "\f212";
}
.fa-sellsy:before {
  content: "\f213";
}
.fa-shirtsinbulk:before {
  content: "\f214";
}
.fa-simplybuilt:before {
  content: "\f215";
}
.fa-skyatlas:before {
  content: "\f216";
}
.fa-cart-plus:before {
  content: "\f217";
}
.fa-cart-arrow-down:before {
  content: "\f218";
}
.fa-diamond:before {
  content: "\f219";
}
.fa-ship:before {
  content: "\f21a";
}
.fa-user-secret:before {
  content: "\f21b";
}
.fa-motorcycle:before {
  content: "\f21c";
}
.fa-street-view:before {
  content: "\f21d";
}
.fa-heartbeat:before {
  content: "\f21e";
}
.fa-venus:before {
  content: "\f221";
}
.fa-mars:before {
  content: "\f222";
}
.fa-mercury:before {
  content: "\f223";
}
.fa-intersex:before,
.fa-transgender:before {
  content: "\f224";
}
.fa-transgender-alt:before {
  content: "\f225";
}
.fa-venus-double:before {
  content: "\f226";
}
.fa-mars-double:before {
  content: "\f227";
}
.fa-venus-mars:before {
  content: "\f228";
}
.fa-mars-stroke:before {
  content: "\f229";
}
.fa-mars-stroke-v:before {
  content: "\f22a";
}
.fa-mars-stroke-h:before {
  content: "\f22b";
}
.fa-neuter:before {
  content: "\f22c";
}
.fa-genderless:before {
  content: "\f22d";
}
.fa-facebook-official:before {
  content: "\f230";
}
.fa-pinterest-p:before {
  content: "\f231";
}
.fa-whatsapp:before {
  content: "\f232";
}
.fa-server:before {
  content: "\f233";
}
.fa-user-plus:before {
  content: "\f234";
}
.fa-user-times:before {
  content: "\f235";
}
.fa-hotel:before,
.fa-bed:before {
  content: "\f236";
}
.fa-viacoin:before {
  content: "\f237";
}
.fa-train:before {
  content: "\f238";
}
.fa-subway:before {
  content: "\f239";
}
.fa-medium:before {
  content: "\f23a";
}
.fa-yc:before,
.fa-y-combinator:before {
  content: "\f23b";
}
.fa-optin-monster:before {
  content: "\f23c";
}
.fa-opencart:before {
  content: "\f23d";
}
.fa-expeditedssl:before {
  content: "\f23e";
}
.fa-battery-4:before,
.fa-battery:before,
.fa-battery-full:before {
  content: "\f240";
}
.fa-battery-3:before,
.fa-battery-three-quarters:before {
  content: "\f241";
}
.fa-battery-2:before,
.fa-battery-half:before {
  content: "\f242";
}
.fa-battery-1:before,
.fa-battery-quarter:before {
  content: "\f243";
}
.fa-battery-0:before,
.fa-battery-empty:before {
  content: "\f244";
}
.fa-mouse-pointer:before {
  content: "\f245";
}
.fa-i-cursor:before {
  content: "\f246";
}
.fa-object-group:before {
  content: "\f247";
}
.fa-object-ungroup:before {
  content: "\f248";
}
.fa-sticky-note:before {
  content: "\f249";
}
.fa-sticky-note-o:before {
  content: "\f24a";
}
.fa-cc-jcb:before {
  content: "\f24b";
}
.fa-cc-diners-club:before {
  content: "\f24c";
}
.fa-clone:before {
  content: "\f24d";
}
.fa-balance-scale:before {
  content: "\f24e";
}
.fa-hourglass-o:before {
  content: "\f250";
}
.fa-hourglass-1:before,
.fa-hourglass-start:before {
  content: "\f251";
}
.fa-hourglass-2:before,
.fa-hourglass-half:before {
  content: "\f252";
}
.fa-hourglass-3:before,
.fa-hourglass-end:before {
  content: "\f253";
}
.fa-hourglass:before {
  content: "\f254";
}
.fa-hand-grab-o:before,
.fa-hand-rock-o:before {
  content: "\f255";
}
.fa-hand-stop-o:before,
.fa-hand-paper-o:before {
  content: "\f256";
}
.fa-hand-scissors-o:before {
  content: "\f257";
}
.fa-hand-lizard-o:before {
  content: "\f258";
}
.fa-hand-spock-o:before {
  content: "\f259";
}
.fa-hand-pointer-o:before {
  content: "\f25a";
}
.fa-hand-peace-o:before {
  content: "\f25b";
}
.fa-trademark:before {
  content: "\f25c";
}
.fa-registered:before {
  content: "\f25d";
}
.fa-creative-commons:before {
  content: "\f25e";
}
.fa-gg:before {
  content: "\f260";
}
.fa-gg-circle:before {
  content: "\f261";
}
.fa-tripadvisor:before {
  content: "\f262";
}
.fa-odnoklassniki:before {
  content: "\f263";
}
.fa-odnoklassniki-square:before {
  content: "\f264";
}
.fa-get-pocket:before {
  content: "\f265";
}
.fa-wikipedia-w:before {
  content: "\f266";
}
.fa-safari:before {
  content: "\f267";
}
.fa-chrome:before {
  content: "\f268";
}
.fa-firefox:before {
  content: "\f269";
}
.fa-opera:before {
  content: "\f26a";
}
.fa-internet-explorer:before {
  content: "\f26b";
}
.fa-tv:before,
.fa-television:before {
  content: "\f26c";
}
.fa-contao:before {
  content: "\f26d";
}
.fa-500px:before {
  content: "\f26e";
}
.fa-amazon:before {
  content: "\f270";
}
.fa-calendar-plus-o:before {
  content: "\f271";
}
.fa-calendar-minus-o:before {
  content: "\f272";
}
.fa-calendar-times-o:before {
  content: "\f273";
}
.fa-calendar-check-o:before {
  content: "\f274";
}
.fa-industry:before {
  content: "\f275";
}
.fa-map-pin:before {
  content: "\f276";
}
.fa-map-signs:before {
  content: "\f277";
}
.fa-map-o:before {
  content: "\f278";
}
.fa-map:before {
  content: "\f279";
}
.fa-commenting:before {
  content: "\f27a";
}
.fa-commenting-o:before {
  content: "\f27b";
}
.fa-houzz:before {
  content: "\f27c";
}
.fa-vimeo:before {
  content: "\f27d";
}
.fa-black-tie:before {
  content: "\f27e";
}
.fa-fonticons:before {
  content: "\f280";
}
.fa-reddit-alien:before {
  content: "\f281";
}
.fa-edge:before {
  content: "\f282";
}
.fa-credit-card-alt:before {
  content: "\f283";
}
.fa-codiepie:before {
  content: "\f284";
}
.fa-modx:before {
  content: "\f285";
}
.fa-fort-awesome:before {
  content: "\f286";
}
.fa-usb:before {
  content: "\f287";
}
.fa-product-hunt:before {
  content: "\f288";
}
.fa-mixcloud:before {
  content: "\f289";
}
.fa-scribd:before {
  content: "\f28a";
}
.fa-pause-circle:before {
  content: "\f28b";
}
.fa-pause-circle-o:before {
  content: "\f28c";
}
.fa-stop-circle:before {
  content: "\f28d";
}
.fa-stop-circle-o:before {
  content: "\f28e";
}
.fa-shopping-bag:before {
  content: "\f290";
}
.fa-shopping-basket:before {
  content: "\f291";
}
.fa-hashtag:before {
  content: "\f292";
}
.fa-bluetooth:before {
  content: "\f293";
}
.fa-bluetooth-b:before {
  content: "\f294";
}
.fa-percent:before {
  content: "\f295";
}
.fa-gitlab:before {
  content: "\f296";
}
.fa-wpbeginner:before {
  content: "\f297";
}
.fa-wpforms:before {
  content: "\f298";
}
.fa-envira:before {
  content: "\f299";
}
.fa-universal-access:before {
  content: "\f29a";
}
.fa-wheelchair-alt:before {
  content: "\f29b";
}
.fa-question-circle-o:before {
  content: "\f29c";
}
.fa-blind:before {
  content: "\f29d";
}
.fa-audio-description:before {
  content: "\f29e";
}
.fa-volume-control-phone:before {
  content: "\f2a0";
}
.fa-braille:before {
  content: "\f2a1";
}
.fa-assistive-listening-systems:before {
  content: "\f2a2";
}
.fa-asl-interpreting:before,
.fa-american-sign-language-interpreting:before {
  content: "\f2a3";
}
.fa-deafness:before,
.fa-hard-of-hearing:before,
.fa-deaf:before {
  content: "\f2a4";
}
.fa-glide:before {
  content: "\f2a5";
}
.fa-glide-g:before {
  content: "\f2a6";
}
.fa-signing:before,
.fa-sign-language:before {
  content: "\f2a7";
}
.fa-low-vision:before {
  content: "\f2a8";
}
.fa-viadeo:before {
  content: "\f2a9";
}
.fa-viadeo-square:before {
  content: "\f2aa";
}
.fa-snapchat:before {
  content: "\f2ab";
}
.fa-snapchat-ghost:before {
  content: "\f2ac";
}
.fa-snapchat-square:before {
  content: "\f2ad";
}
.fa-pied-piper:before {
  content: "\f2ae";
}
.fa-first-order:before {
  content: "\f2b0";
}
.fa-yoast:before {
  content: "\f2b1";
}
.fa-themeisle:before {
  content: "\f2b2";
}
.fa-google-plus-circle:before,
.fa-google-plus-official:before {
  content: "\f2b3";
}
.fa-fa:before,
.fa-font-awesome:before {
  content: "\f2b4";
}
.fa-handshake-o:before {
  content: "\f2b5";
}
.fa-envelope-open:before {
  content: "\f2b6";
}
.fa-envelope-open-o:before {
  content: "\f2b7";
}
.fa-linode:before {
  content: "\f2b8";
}
.fa-address-book:before {
  content: "\f2b9";
}
.fa-address-book-o:before {
  content: "\f2ba";
}
.fa-vcard:before,
.fa-address-card:before {
  content: "\f2bb";
}
.fa-vcard-o:before,
.fa-address-card-o:before {
  content: "\f2bc";
}
.fa-user-circle:before {
  content: "\f2bd";
}
.fa-user-circle-o:before {
  content: "\f2be";
}
.fa-user-o:before {
  content: "\f2c0";
}
.fa-id-badge:before {
  content: "\f2c1";
}
.fa-drivers-license:before,
.fa-id-card:before {
  content: "\f2c2";
}
.fa-drivers-license-o:before,
.fa-id-card-o:before {
  content: "\f2c3";
}
.fa-quora:before {
  content: "\f2c4";
}
.fa-free-code-camp:before {
  content: "\f2c5";
}
.fa-telegram:before {
  content: "\f2c6";
}
.fa-thermometer-4:before,
.fa-thermometer:before,
.fa-thermometer-full:before {
  content: "\f2c7";
}
.fa-thermometer-3:before,
.fa-thermometer-three-quarters:before {
  content: "\f2c8";
}
.fa-thermometer-2:before,
.fa-thermometer-half:before {
  content: "\f2c9";
}
.fa-thermometer-1:before,
.fa-thermometer-quarter:before {
  content: "\f2ca";
}
.fa-thermometer-0:before,
.fa-thermometer-empty:before {
  content: "\f2cb";
}
.fa-shower:before {
  content: "\f2cc";
}
.fa-bathtub:before,
.fa-s15:before,
.fa-bath:before {
  content: "\f2cd";
}
.fa-podcast:before {
  content: "\f2ce";
}
.fa-window-maximize:before {
  content: "\f2d0";
}
.fa-window-minimize:before {
  content: "\f2d1";
}
.fa-window-restore:before {
  content: "\f2d2";
}
.fa-times-rectangle:before,
.fa-window-close:before {
  content: "\f2d3";
}
.fa-times-rectangle-o:before,
.fa-window-close-o:before {
  content: "\f2d4";
}
.fa-bandcamp:before {
  content: "\f2d5";
}
.fa-grav:before {
  content: "\f2d6";
}
.fa-etsy:before {
  content: "\f2d7";
}
.fa-imdb:before {
  content: "\f2d8";
}
.fa-ravelry:before {
  content: "\f2d9";
}
.fa-eercast:before {
  content: "\f2da";
}
.fa-microchip:before {
  content: "\f2db";
}
.fa-snowflake-o:before {
  content: "\f2dc";
}
.fa-superpowers:before {
  content: "\f2dd";
}
.fa-wpexplorer:before {
  content: "\f2de";
}
.fa-meetup:before {
  content: "\f2e0";
}
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
.sr-only-focusable:active,
.sr-only-focusable:focus {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
}
/*!
*
* IPython base
*
*/
.modal.fade .modal-dialog {
  -webkit-transform: translate(0, 0);
  -ms-transform: translate(0, 0);
  -o-transform: translate(0, 0);
  transform: translate(0, 0);
}
code {
  color: #000;
}
pre {
  font-size: inherit;
  line-height: inherit;
}
label {
  font-weight: normal;
}
/* Make the page background atleast 100% the height of the view port */
/* Make the page itself atleast 70% the height of the view port */
.border-box-sizing {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.corner-all {
  border-radius: 2px;
}
.no-padding {
  padding: 0px;
}
/* Flexible box model classes */
/* Taken from Alex Russell http://infrequently.org/2009/08/css-3-progress/ */
/* This file is a compatability layer.  It allows the usage of flexible box 
model layouts accross multiple browsers, including older browsers.  The newest,
universal implementation of the flexible box model is used when available (see
`Modern browsers` comments below).  Browsers that are known to implement this 
new spec completely include:

    Firefox 28.0+
    Chrome 29.0+
    Internet Explorer 11+ 
    Opera 17.0+

Browsers not listed, including Safari, are supported via the styling under the
`Old browsers` comments below.
*/
.hbox {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
.hbox > * {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
}
.vbox {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
.vbox > * {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
}
.hbox.reverse,
.vbox.reverse,
.reverse {
  /* Old browsers */
  -webkit-box-direction: reverse;
  -moz-box-direction: reverse;
  box-direction: reverse;
  /* Modern browsers */
  flex-direction: row-reverse;
}
.hbox.box-flex0,
.vbox.box-flex0,
.box-flex0 {
  /* Old browsers */
  -webkit-box-flex: 0;
  -moz-box-flex: 0;
  box-flex: 0;
  /* Modern browsers */
  flex: none;
  width: auto;
}
.hbox.box-flex1,
.vbox.box-flex1,
.box-flex1 {
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
.hbox.box-flex,
.vbox.box-flex,
.box-flex {
  /* Old browsers */
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
.hbox.box-flex2,
.vbox.box-flex2,
.box-flex2 {
  /* Old browsers */
  -webkit-box-flex: 2;
  -moz-box-flex: 2;
  box-flex: 2;
  /* Modern browsers */
  flex: 2;
}
.box-group1 {
  /*  Deprecated */
  -webkit-box-flex-group: 1;
  -moz-box-flex-group: 1;
  box-flex-group: 1;
}
.box-group2 {
  /* Deprecated */
  -webkit-box-flex-group: 2;
  -moz-box-flex-group: 2;
  box-flex-group: 2;
}
.hbox.start,
.vbox.start,
.start {
  /* Old browsers */
  -webkit-box-pack: start;
  -moz-box-pack: start;
  box-pack: start;
  /* Modern browsers */
  justify-content: flex-start;
}
.hbox.end,
.vbox.end,
.end {
  /* Old browsers */
  -webkit-box-pack: end;
  -moz-box-pack: end;
  box-pack: end;
  /* Modern browsers */
  justify-content: flex-end;
}
.hbox.center,
.vbox.center,
.center {
  /* Old browsers */
  -webkit-box-pack: center;
  -moz-box-pack: center;
  box-pack: center;
  /* Modern browsers */
  justify-content: center;
}
.hbox.baseline,
.vbox.baseline,
.baseline {
  /* Old browsers */
  -webkit-box-pack: baseline;
  -moz-box-pack: baseline;
  box-pack: baseline;
  /* Modern browsers */
  justify-content: baseline;
}
.hbox.stretch,
.vbox.stretch,
.stretch {
  /* Old browsers */
  -webkit-box-pack: stretch;
  -moz-box-pack: stretch;
  box-pack: stretch;
  /* Modern browsers */
  justify-content: stretch;
}
.hbox.align-start,
.vbox.align-start,
.align-start {
  /* Old browsers */
  -webkit-box-align: start;
  -moz-box-align: start;
  box-align: start;
  /* Modern browsers */
  align-items: flex-start;
}
.hbox.align-end,
.vbox.align-end,
.align-end {
  /* Old browsers */
  -webkit-box-align: end;
  -moz-box-align: end;
  box-align: end;
  /* Modern browsers */
  align-items: flex-end;
}
.hbox.align-center,
.vbox.align-center,
.align-center {
  /* Old browsers */
  -webkit-box-align: center;
  -moz-box-align: center;
  box-align: center;
  /* Modern browsers */
  align-items: center;
}
.hbox.align-baseline,
.vbox.align-baseline,
.align-baseline {
  /* Old browsers */
  -webkit-box-align: baseline;
  -moz-box-align: baseline;
  box-align: baseline;
  /* Modern browsers */
  align-items: baseline;
}
.hbox.align-stretch,
.vbox.align-stretch,
.align-stretch {
  /* Old browsers */
  -webkit-box-align: stretch;
  -moz-box-align: stretch;
  box-align: stretch;
  /* Modern browsers */
  align-items: stretch;
}
div.error {
  margin: 2em;
  text-align: center;
}
div.error > h1 {
  font-size: 500%;
  line-height: normal;
}
div.error > p {
  font-size: 200%;
  line-height: normal;
}
div.traceback-wrapper {
  text-align: left;
  max-width: 800px;
  margin: auto;
}
div.traceback-wrapper pre.traceback {
  max-height: 600px;
  overflow: auto;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
body {
  background-color: #fff;
  /* This makes sure that the body covers the entire window and needs to
       be in a different element than the display: box in wrapper below */
  position: absolute;
  left: 0px;
  right: 0px;
  top: 0px;
  bottom: 0px;
  overflow: visible;
}
body > #header {
  /* Initially hidden to prevent FLOUC */
  display: none;
  background-color: #fff;
  /* Display over codemirror */
  position: relative;
  z-index: 100;
}
body > #header #header-container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 5px;
  padding-bottom: 5px;
  padding-top: 5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
body > #header .header-bar {
  width: 100%;
  height: 1px;
  background: #e7e7e7;
  margin-bottom: -1px;
}
@media print {
  body > #header {
    display: none !important;
  }
}
#header-spacer {
  width: 100%;
  visibility: hidden;
}
@media print {
  #header-spacer {
    display: none;
  }
}
#ipython_notebook {
  padding-left: 0px;
  padding-top: 1px;
  padding-bottom: 1px;
}
[dir="rtl"] #ipython_notebook {
  margin-right: 10px;
  margin-left: 0;
}
[dir="rtl"] #ipython_notebook.pull-left {
  float: right !important;
  float: right;
}
.flex-spacer {
  flex: 1;
}
#noscript {
  width: auto;
  padding-top: 16px;
  padding-bottom: 16px;
  text-align: center;
  font-size: 22px;
  color: red;
  font-weight: bold;
}
#ipython_notebook img {
  height: 28px;
}
#site {
  width: 100%;
  display: none;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  overflow: auto;
}
@media print {
  #site {
    height: auto !important;
  }
}
/* Smaller buttons */
.ui-button .ui-button-text {
  padding: 0.2em 0.8em;
  font-size: 77%;
}
input.ui-button {
  padding: 0.3em 0.9em;
}
span#kernel_logo_widget {
  margin: 0 10px;
}
span#login_widget {
  float: right;
}
[dir="rtl"] span#login_widget {
  float: left;
}
span#login_widget > .button,
#logout {
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
span#login_widget > .button:focus,
#logout:focus,
span#login_widget > .button.focus,
#logout.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
span#login_widget > .button:hover,
#logout:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
span#login_widget > .button:active,
#logout:active,
span#login_widget > .button.active,
#logout.active,
.open > .dropdown-togglespan#login_widget > .button,
.open > .dropdown-toggle#logout {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
span#login_widget > .button:active:hover,
#logout:active:hover,
span#login_widget > .button.active:hover,
#logout.active:hover,
.open > .dropdown-togglespan#login_widget > .button:hover,
.open > .dropdown-toggle#logout:hover,
span#login_widget > .button:active:focus,
#logout:active:focus,
span#login_widget > .button.active:focus,
#logout.active:focus,
.open > .dropdown-togglespan#login_widget > .button:focus,
.open > .dropdown-toggle#logout:focus,
span#login_widget > .button:active.focus,
#logout:active.focus,
span#login_widget > .button.active.focus,
#logout.active.focus,
.open > .dropdown-togglespan#login_widget > .button.focus,
.open > .dropdown-toggle#logout.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
span#login_widget > .button:active,
#logout:active,
span#login_widget > .button.active,
#logout.active,
.open > .dropdown-togglespan#login_widget > .button,
.open > .dropdown-toggle#logout {
  background-image: none;
}
span#login_widget > .button.disabled:hover,
#logout.disabled:hover,
span#login_widget > .button[disabled]:hover,
#logout[disabled]:hover,
fieldset[disabled] span#login_widget > .button:hover,
fieldset[disabled] #logout:hover,
span#login_widget > .button.disabled:focus,
#logout.disabled:focus,
span#login_widget > .button[disabled]:focus,
#logout[disabled]:focus,
fieldset[disabled] span#login_widget > .button:focus,
fieldset[disabled] #logout:focus,
span#login_widget > .button.disabled.focus,
#logout.disabled.focus,
span#login_widget > .button[disabled].focus,
#logout[disabled].focus,
fieldset[disabled] span#login_widget > .button.focus,
fieldset[disabled] #logout.focus {
  background-color: #fff;
  border-color: #ccc;
}
span#login_widget > .button .badge,
#logout .badge {
  color: #fff;
  background-color: #333;
}
.nav-header {
  text-transform: none;
}
#header > span {
  margin-top: 10px;
}
.modal_stretch .modal-dialog {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  min-height: 80vh;
}
.modal_stretch .modal-dialog .modal-body {
  max-height: calc(100vh - 200px);
  overflow: auto;
  flex: 1;
}
.modal-header {
  cursor: move;
}
@media (min-width: 768px) {
  .modal .modal-dialog {
    width: 700px;
  }
}
@media (min-width: 768px) {
  select.form-control {
    margin-left: 12px;
    margin-right: 12px;
  }
}
/*!
*
* IPython auth
*
*/
.center-nav {
  display: inline-block;
  margin-bottom: -4px;
}
[dir="rtl"] .center-nav form.pull-left {
  float: right !important;
  float: right;
}
[dir="rtl"] .center-nav .navbar-text {
  float: right;
}
[dir="rtl"] .navbar-inner {
  text-align: right;
}
[dir="rtl"] div.text-left {
  text-align: right;
}
/*!
*
* IPython tree view
*
*/
/* We need an invisible input field on top of the sentense*/
/* "Drag file onto the list ..." */
.alternate_upload {
  background-color: none;
  display: inline;
}
.alternate_upload.form {
  padding: 0;
  margin: 0;
}
.alternate_upload input.fileinput {
  position: absolute;
  display: block;
  width: 100%;
  height: 100%;
  overflow: hidden;
  cursor: pointer;
  opacity: 0;
  z-index: 2;
}
.alternate_upload .btn-xs > input.fileinput {
  margin: -1px -5px;
}
.alternate_upload .btn-upload {
  position: relative;
  height: 22px;
}
::-webkit-file-upload-button {
  cursor: pointer;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
ul#tabs {
  margin-bottom: 4px;
}
ul#tabs a {
  padding-top: 6px;
  padding-bottom: 4px;
}
[dir="rtl"] ul#tabs.nav-tabs > li {
  float: right;
}
[dir="rtl"] ul#tabs.nav.nav-tabs {
  padding-right: 0;
}
ul.breadcrumb a:focus,
ul.breadcrumb a:hover {
  text-decoration: none;
}
ul.breadcrumb i.icon-home {
  font-size: 16px;
  margin-right: 4px;
}
ul.breadcrumb span {
  color: #5e5e5e;
}
.list_toolbar {
  padding: 4px 0 4px 0;
  vertical-align: middle;
}
.list_toolbar .tree-buttons {
  padding-top: 1px;
}
[dir="rtl"] .list_toolbar .tree-buttons .pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .list_toolbar .col-sm-4,
[dir="rtl"] .list_toolbar .col-sm-8 {
  float: right;
}
.dynamic-buttons {
  padding-top: 3px;
  display: inline-block;
}
.list_toolbar [class*="span"] {
  min-height: 24px;
}
.list_header {
  font-weight: bold;
  background-color: #EEE;
}
.list_placeholder {
  font-weight: bold;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
}
.list_container {
  margin-top: 4px;
  margin-bottom: 20px;
  border: 1px solid #ddd;
  border-radius: 2px;
}
.list_container > div {
  border-bottom: 1px solid #ddd;
}
.list_container > div:hover .list-item {
  background-color: red;
}
.list_container > div:last-child {
  border: none;
}
.list_item:hover .list_item {
  background-color: #ddd;
}
.list_item a {
  text-decoration: none;
}
.list_item:hover {
  background-color: #fafafa;
}
.list_header > div,
.list_item > div {
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
  line-height: 22px;
}
.list_header > div input,
.list_item > div input {
  margin-right: 7px;
  margin-left: 14px;
  vertical-align: text-bottom;
  line-height: 22px;
  position: relative;
  top: -1px;
}
.list_header > div .item_link,
.list_item > div .item_link {
  margin-left: -1px;
  vertical-align: baseline;
  line-height: 22px;
}
[dir="rtl"] .list_item > div input {
  margin-right: 0;
}
.new-file input[type=checkbox] {
  visibility: hidden;
}
.item_name {
  line-height: 22px;
  height: 24px;
}
.item_icon {
  font-size: 14px;
  color: #5e5e5e;
  margin-right: 7px;
  margin-left: 7px;
  line-height: 22px;
  vertical-align: baseline;
}
.item_modified {
  margin-right: 7px;
  margin-left: 7px;
}
[dir="rtl"] .item_modified.pull-right {
  float: left !important;
  float: left;
}
.item_buttons {
  line-height: 1em;
  margin-left: -5px;
}
.item_buttons .btn,
.item_buttons .btn-group,
.item_buttons .input-group {
  float: left;
}
.item_buttons > .btn,
.item_buttons > .btn-group,
.item_buttons > .input-group {
  margin-left: 5px;
}
.item_buttons .btn {
  min-width: 13ex;
}
.item_buttons .running-indicator {
  padding-top: 4px;
  color: #5cb85c;
}
.item_buttons .kernel-name {
  padding-top: 4px;
  color: #5bc0de;
  margin-right: 7px;
  float: left;
}
[dir="rtl"] .item_buttons.pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .item_buttons .kernel-name {
  margin-left: 7px;
  float: right;
}
.toolbar_info {
  height: 24px;
  line-height: 24px;
}
.list_item input:not([type=checkbox]) {
  padding-top: 3px;
  padding-bottom: 3px;
  height: 22px;
  line-height: 14px;
  margin: 0px;
}
.highlight_text {
  color: blue;
}
#project_name {
  display: inline-block;
  padding-left: 7px;
  margin-left: -2px;
}
#project_name > .breadcrumb {
  padding: 0px;
  margin-bottom: 0px;
  background-color: transparent;
  font-weight: bold;
}
.sort_button {
  display: inline-block;
  padding-left: 7px;
}
[dir="rtl"] .sort_button.pull-right {
  float: left !important;
  float: left;
}
#tree-selector {
  padding-right: 0px;
}
#button-select-all {
  min-width: 50px;
}
[dir="rtl"] #button-select-all.btn {
  float: right ;
}
#select-all {
  margin-left: 7px;
  margin-right: 2px;
  margin-top: 2px;
  height: 16px;
}
[dir="rtl"] #select-all.pull-left {
  float: right !important;
  float: right;
}
.menu_icon {
  margin-right: 2px;
}
.tab-content .row {
  margin-left: 0px;
  margin-right: 0px;
}
.folder_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f114";
}
.folder_icon:before.fa-pull-left {
  margin-right: .3em;
}
.folder_icon:before.fa-pull-right {
  margin-left: .3em;
}
.folder_icon:before.pull-left {
  margin-right: .3em;
}
.folder_icon:before.pull-right {
  margin-left: .3em;
}
.notebook_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f02d";
  position: relative;
  top: -1px;
}
.notebook_icon:before.fa-pull-left {
  margin-right: .3em;
}
.notebook_icon:before.fa-pull-right {
  margin-left: .3em;
}
.notebook_icon:before.pull-left {
  margin-right: .3em;
}
.notebook_icon:before.pull-right {
  margin-left: .3em;
}
.running_notebook_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f02d";
  position: relative;
  top: -1px;
  color: #5cb85c;
}
.running_notebook_icon:before.fa-pull-left {
  margin-right: .3em;
}
.running_notebook_icon:before.fa-pull-right {
  margin-left: .3em;
}
.running_notebook_icon:before.pull-left {
  margin-right: .3em;
}
.running_notebook_icon:before.pull-right {
  margin-left: .3em;
}
.file_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f016";
  position: relative;
  top: -2px;
}
.file_icon:before.fa-pull-left {
  margin-right: .3em;
}
.file_icon:before.fa-pull-right {
  margin-left: .3em;
}
.file_icon:before.pull-left {
  margin-right: .3em;
}
.file_icon:before.pull-right {
  margin-left: .3em;
}
#notebook_toolbar .pull-right {
  padding-top: 0px;
  margin-right: -1px;
}
ul#new-menu {
  left: auto;
  right: 0;
}
#new-menu .dropdown-header {
  font-size: 10px;
  border-bottom: 1px solid #e5e5e5;
  padding: 0 0 3px;
  margin: -3px 20px 0;
}
.kernel-menu-icon {
  padding-right: 12px;
  width: 24px;
  content: "\f096";
}
.kernel-menu-icon:before {
  content: "\f096";
}
.kernel-menu-icon-current:before {
  content: "\f00c";
}
#tab_content {
  padding-top: 20px;
}
#running .panel-group .panel {
  margin-top: 3px;
  margin-bottom: 1em;
}
#running .panel-group .panel .panel-heading {
  background-color: #EEE;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 7px;
  padding-right: 7px;
  line-height: 22px;
}
#running .panel-group .panel .panel-heading a:focus,
#running .panel-group .panel .panel-heading a:hover {
  text-decoration: none;
}
#running .panel-group .panel .panel-body {
  padding: 0px;
}
#running .panel-group .panel .panel-body .list_container {
  margin-top: 0px;
  margin-bottom: 0px;
  border: 0px;
  border-radius: 0px;
}
#running .panel-group .panel .panel-body .list_container .list_item {
  border-bottom: 1px solid #ddd;
}
#running .panel-group .panel .panel-body .list_container .list_item:last-child {
  border-bottom: 0px;
}
.delete-button {
  display: none;
}
.duplicate-button {
  display: none;
}
.rename-button {
  display: none;
}
.move-button {
  display: none;
}
.download-button {
  display: none;
}
.shutdown-button {
  display: none;
}
.dynamic-instructions {
  display: inline-block;
  padding-top: 4px;
}
/*!
*
* IPython text editor webapp
*
*/
.selected-keymap i.fa {
  padding: 0px 5px;
}
.selected-keymap i.fa:before {
  content: "\f00c";
}
#mode-menu {
  overflow: auto;
  max-height: 20em;
}
.edit_app #header {
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
.edit_app #menubar .navbar {
  /* Use a negative 1 bottom margin, so the border overlaps the border of the
    header */
  margin-bottom: -1px;
}
.dirty-indicator {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator.pull-left {
  margin-right: .3em;
}
.dirty-indicator.pull-right {
  margin-left: .3em;
}
.dirty-indicator-dirty {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator-dirty.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator-dirty.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator-dirty.pull-left {
  margin-right: .3em;
}
.dirty-indicator-dirty.pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 20px;
}
.dirty-indicator-clean.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean.pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean.pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f00c";
}
.dirty-indicator-clean:before.fa-pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean:before.fa-pull-right {
  margin-left: .3em;
}
.dirty-indicator-clean:before.pull-left {
  margin-right: .3em;
}
.dirty-indicator-clean:before.pull-right {
  margin-left: .3em;
}
#filename {
  font-size: 16pt;
  display: table;
  padding: 0px 5px;
}
#current-mode {
  padding-left: 5px;
  padding-right: 5px;
}
#texteditor-backdrop {
  padding-top: 20px;
  padding-bottom: 20px;
}
@media not print {
  #texteditor-backdrop {
    background-color: #EEE;
  }
}
@media print {
  #texteditor-backdrop #texteditor-container .CodeMirror-gutter,
  #texteditor-backdrop #texteditor-container .CodeMirror-gutters {
    background-color: #fff;
  }
}
@media not print {
  #texteditor-backdrop #texteditor-container .CodeMirror-gutter,
  #texteditor-backdrop #texteditor-container .CodeMirror-gutters {
    background-color: #fff;
  }
}
@media not print {
  #texteditor-backdrop #texteditor-container {
    padding: 0px;
    background-color: #fff;
    -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
    box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  }
}
.CodeMirror-dialog {
  background-color: #fff;
}
/*!
*
* IPython notebook
*
*/
/* CSS font colors for translated ANSI escape sequences */
/* The color values are a mix of
   http://www.xcolors.net/dl/baskerville-ivorylight and
   http://www.xcolors.net/dl/euphrasia */
.ansi-black-fg {
  color: #3E424D;
}
.ansi-black-bg {
  background-color: #3E424D;
}
.ansi-black-intense-fg {
  color: #282C36;
}
.ansi-black-intense-bg {
  background-color: #282C36;
}
.ansi-red-fg {
  color: #E75C58;
}
.ansi-red-bg {
  background-color: #E75C58;
}
.ansi-red-intense-fg {
  color: #B22B31;
}
.ansi-red-intense-bg {
  background-color: #B22B31;
}
.ansi-green-fg {
  color: #00A250;
}
.ansi-green-bg {
  background-color: #00A250;
}
.ansi-green-intense-fg {
  color: #007427;
}
.ansi-green-intense-bg {
  background-color: #007427;
}
.ansi-yellow-fg {
  color: #DDB62B;
}
.ansi-yellow-bg {
  background-color: #DDB62B;
}
.ansi-yellow-intense-fg {
  color: #B27D12;
}
.ansi-yellow-intense-bg {
  background-color: #B27D12;
}
.ansi-blue-fg {
  color: #208FFB;
}
.ansi-blue-bg {
  background-color: #208FFB;
}
.ansi-blue-intense-fg {
  color: #0065CA;
}
.ansi-blue-intense-bg {
  background-color: #0065CA;
}
.ansi-magenta-fg {
  color: #D160C4;
}
.ansi-magenta-bg {
  background-color: #D160C4;
}
.ansi-magenta-intense-fg {
  color: #A03196;
}
.ansi-magenta-intense-bg {
  background-color: #A03196;
}
.ansi-cyan-fg {
  color: #60C6C8;
}
.ansi-cyan-bg {
  background-color: #60C6C8;
}
.ansi-cyan-intense-fg {
  color: #258F8F;
}
.ansi-cyan-intense-bg {
  background-color: #258F8F;
}
.ansi-white-fg {
  color: #C5C1B4;
}
.ansi-white-bg {
  background-color: #C5C1B4;
}
.ansi-white-intense-fg {
  color: #A1A6B2;
}
.ansi-white-intense-bg {
  background-color: #A1A6B2;
}
.ansi-default-inverse-fg {
  color: #FFFFFF;
}
.ansi-default-inverse-bg {
  background-color: #000000;
}
.ansi-bold {
  font-weight: bold;
}
.ansi-underline {
  text-decoration: underline;
}
/* The following styles are deprecated an will be removed in a future version */
.ansibold {
  font-weight: bold;
}
.ansi-inverse {
  outline: 0.5px dotted;
}
/* use dark versions for foreground, to improve visibility */
.ansiblack {
  color: black;
}
.ansired {
  color: darkred;
}
.ansigreen {
  color: darkgreen;
}
.ansiyellow {
  color: #c4a000;
}
.ansiblue {
  color: darkblue;
}
.ansipurple {
  color: darkviolet;
}
.ansicyan {
  color: steelblue;
}
.ansigray {
  color: gray;
}
/* and light for background, for the same reason */
.ansibgblack {
  background-color: black;
}
.ansibgred {
  background-color: red;
}
.ansibggreen {
  background-color: green;
}
.ansibgyellow {
  background-color: yellow;
}
.ansibgblue {
  background-color: blue;
}
.ansibgpurple {
  background-color: magenta;
}
.ansibgcyan {
  background-color: cyan;
}
.ansibggray {
  background-color: gray;
}
div.cell {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  border-radius: 2px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  border-width: 1px;
  border-style: solid;
  border-color: transparent;
  width: 100%;
  padding: 5px;
  /* This acts as a spacer between cells, that is outside the border */
  margin: 0px;
  outline: none;
  position: relative;
  overflow: visible;
}
div.cell:before {
  position: absolute;
  display: block;
  top: -1px;
  left: -1px;
  width: 5px;
  height: calc(100% +  2px);
  content: '';
  background: transparent;
}
div.cell.jupyter-soft-selected {
  border-left-color: #E3F2FD;
  border-left-width: 1px;
  padding-left: 5px;
  border-right-color: #E3F2FD;
  border-right-width: 1px;
  background: #E3F2FD;
}
@media print {
  div.cell.jupyter-soft-selected {
    border-color: transparent;
  }
}
div.cell.selected,
div.cell.selected.jupyter-soft-selected {
  border-color: #ababab;
}
div.cell.selected:before,
div.cell.selected.jupyter-soft-selected:before {
  position: absolute;
  display: block;
  top: -1px;
  left: -1px;
  width: 5px;
  height: calc(100% +  2px);
  content: '';
  background: #42A5F5;
}
@media print {
  div.cell.selected,
  div.cell.selected.jupyter-soft-selected {
    border-color: transparent;
  }
}
.edit_mode div.cell.selected {
  border-color: #66BB6A;
}
.edit_mode div.cell.selected:before {
  position: absolute;
  display: block;
  top: -1px;
  left: -1px;
  width: 5px;
  height: calc(100% +  2px);
  content: '';
  background: #66BB6A;
}
@media print {
  .edit_mode div.cell.selected {
    border-color: transparent;
  }
}
.prompt {
  /* This needs to be wide enough for 3 digit prompt numbers: In[100]: */
  min-width: 14ex;
  /* This padding is tuned to match the padding on the CodeMirror editor. */
  padding: 0.4em;
  margin: 0px;
  font-family: monospace;
  text-align: right;
  /* This has to match that of the the CodeMirror class line-height below */
  line-height: 1.21429em;
  /* Don't highlight prompt number selection */
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  /* Use default cursor */
  cursor: default;
}
@media (max-width: 540px) {
  .prompt {
    text-align: left;
  }
}
div.inner_cell {
  min-width: 0;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
/* input_area and input_prompt must match in top border and margin for alignment */
div.input_area {
  border: 1px solid #cfcfcf;
  border-radius: 2px;
  background: #f7f7f7;
  line-height: 1.21429em;
}
/* This is needed so that empty prompt areas can collapse to zero height when there
   is no content in the output_subarea and the prompt. The main purpose of this is
   to make sure that empty JavaScript output_subareas have no height. */
div.prompt:empty {
  padding-top: 0;
  padding-bottom: 0;
}
div.unrecognized_cell {
  padding: 5px 5px 5px 0px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
div.unrecognized_cell .inner_cell {
  border-radius: 2px;
  padding: 5px;
  font-weight: bold;
  color: red;
  border: 1px solid #cfcfcf;
  background: #eaeaea;
}
div.unrecognized_cell .inner_cell a {
  color: inherit;
  text-decoration: none;
}
div.unrecognized_cell .inner_cell a:hover {
  color: inherit;
  text-decoration: none;
}
@media (max-width: 540px) {
  div.unrecognized_cell > div.prompt {
    display: none;
  }
}
div.code_cell {
  /* avoid page breaking on code cells when printing */
}
@media print {
  div.code_cell {
    page-break-inside: avoid;
  }
}
/* any special styling for code cells that are currently running goes here */
div.input {
  page-break-inside: avoid;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.input {
    /* Old browsers */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-box-align: stretch;
    display: -moz-box;
    -moz-box-orient: vertical;
    -moz-box-align: stretch;
    display: box;
    box-orient: vertical;
    box-align: stretch;
    /* Modern browsers */
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }
}
/* input_area and input_prompt must match in top border and margin for alignment */
div.input_prompt {
  color: #303F9F;
  border-top: 1px solid transparent;
}
div.input_area > div.highlight {
  margin: 0.4em;
  border: none;
  padding: 0px;
  background-color: transparent;
}
div.input_area > div.highlight > pre {
  margin: 0px;
  border: none;
  padding: 0px;
  background-color: transparent;
}
/* The following gets added to the <head> if it is detected that the user has a
 * monospace font with inconsistent normal/bold/italic height.  See
 * notebookmain.js.  Such fonts will have keywords vertically offset with
 * respect to the rest of the text.  The user should select a better font.
 * See: https://github.com/ipython/ipython/issues/1503
 *
 * .CodeMirror span {
 *      vertical-align: bottom;
 * }
 */
.CodeMirror {
  line-height: 1.21429em;
  /* Changed from 1em to our global default */
  font-size: 14px;
  height: auto;
  /* Changed to auto to autogrow */
  background: none;
  /* Changed from white to allow our bg to show through */
}
.CodeMirror-scroll {
  /*  The CodeMirror docs are a bit fuzzy on if overflow-y should be hidden or visible.*/
  /*  We have found that if it is visible, vertical scrollbars appear with font size changes.*/
  overflow-y: hidden;
  overflow-x: auto;
}
.CodeMirror-lines {
  /* In CM2, this used to be 0.4em, but in CM3 it went to 4px. We need the em value because */
  /* we have set a different line-height and want this to scale with that. */
  /* Note that this should set vertical padding only, since CodeMirror assumes
       that horizontal padding will be set on CodeMirror pre */
  padding: 0.4em 0;
}
.CodeMirror-linenumber {
  padding: 0 8px 0 4px;
}
.CodeMirror-gutters {
  border-bottom-left-radius: 2px;
  border-top-left-radius: 2px;
}
.CodeMirror pre {
  /* In CM3 this went to 4px from 0 in CM2. This sets horizontal padding only,
    use .CodeMirror-lines for vertical */
  padding: 0 0.4em;
  border: 0;
  border-radius: 0;
}
.CodeMirror-cursor {
  border-left: 1.4px solid black;
}
@media screen and (min-width: 2138px) and (max-width: 4319px) {
  .CodeMirror-cursor {
    border-left: 2px solid black;
  }
}
@media screen and (min-width: 4320px) {
  .CodeMirror-cursor {
    border-left: 4px solid black;
  }
}
/*

Original style from softwaremaniacs.org (c) Ivan Sagalaev <Maniac@SoftwareManiacs.Org>
Adapted from GitHub theme

*/
.highlight-base {
  color: #000;
}
.highlight-variable {
  color: #000;
}
.highlight-variable-2 {
  color: #1a1a1a;
}
.highlight-variable-3 {
  color: #333333;
}
.highlight-string {
  color: #BA2121;
}
.highlight-comment {
  color: #408080;
  font-style: italic;
}
.highlight-number {
  color: #080;
}
.highlight-atom {
  color: #88F;
}
.highlight-keyword {
  color: #008000;
  font-weight: bold;
}
.highlight-builtin {
  color: #008000;
}
.highlight-error {
  color: #f00;
}
.highlight-operator {
  color: #AA22FF;
  font-weight: bold;
}
.highlight-meta {
  color: #AA22FF;
}
/* previously not defined, copying from default codemirror */
.highlight-def {
  color: #00f;
}
.highlight-string-2 {
  color: #f50;
}
.highlight-qualifier {
  color: #555;
}
.highlight-bracket {
  color: #997;
}
.highlight-tag {
  color: #170;
}
.highlight-attribute {
  color: #00c;
}
.highlight-header {
  color: blue;
}
.highlight-quote {
  color: #090;
}
.highlight-link {
  color: #00c;
}
/* apply the same style to codemirror */
.cm-s-ipython span.cm-keyword {
  color: #008000;
  font-weight: bold;
}
.cm-s-ipython span.cm-atom {
  color: #88F;
}
.cm-s-ipython span.cm-number {
  color: #080;
}
.cm-s-ipython span.cm-def {
  color: #00f;
}
.cm-s-ipython span.cm-variable {
  color: #000;
}
.cm-s-ipython span.cm-operator {
  color: #AA22FF;
  font-weight: bold;
}
.cm-s-ipython span.cm-variable-2 {
  color: #1a1a1a;
}
.cm-s-ipython span.cm-variable-3 {
  color: #333333;
}
.cm-s-ipython span.cm-comment {
  color: #408080;
  font-style: italic;
}
.cm-s-ipython span.cm-string {
  color: #BA2121;
}
.cm-s-ipython span.cm-string-2 {
  color: #f50;
}
.cm-s-ipython span.cm-meta {
  color: #AA22FF;
}
.cm-s-ipython span.cm-qualifier {
  color: #555;
}
.cm-s-ipython span.cm-builtin {
  color: #008000;
}
.cm-s-ipython span.cm-bracket {
  color: #997;
}
.cm-s-ipython span.cm-tag {
  color: #170;
}
.cm-s-ipython span.cm-attribute {
  color: #00c;
}
.cm-s-ipython span.cm-header {
  color: blue;
}
.cm-s-ipython span.cm-quote {
  color: #090;
}
.cm-s-ipython span.cm-link {
  color: #00c;
}
.cm-s-ipython span.cm-error {
  color: #f00;
}
.cm-s-ipython span.cm-tab {
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAMCAYAAAAkuj5RAAAAAXNSR0IArs4c6QAAAGFJREFUSMft1LsRQFAQheHPowAKoACx3IgEKtaEHujDjORSgWTH/ZOdnZOcM/sgk/kFFWY0qV8foQwS4MKBCS3qR6ixBJvElOobYAtivseIE120FaowJPN75GMu8j/LfMwNjh4HUpwg4LUAAAAASUVORK5CYII=);
  background-position: right;
  background-repeat: no-repeat;
}
div.output_wrapper {
  /* this position must be relative to enable descendents to be absolute within it */
  position: relative;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
  z-index: 1;
}
/* class for the output area when it should be height-limited */
div.output_scroll {
  /* ideally, this would be max-height, but FF barfs all over that */
  height: 24em;
  /* FF needs this *and the wrapper* to specify full width, or it will shrinkwrap */
  width: 100%;
  overflow: auto;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.8);
  box-shadow: inset 0 2px 8px rgba(0, 0, 0, 0.8);
  display: block;
}
/* output div while it is collapsed */
div.output_collapsed {
  margin: 0px;
  padding: 0px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
div.out_prompt_overlay {
  height: 100%;
  padding: 0px 0.4em;
  position: absolute;
  border-radius: 2px;
}
div.out_prompt_overlay:hover {
  /* use inner shadow to get border that is computed the same on WebKit/FF */
  -webkit-box-shadow: inset 0 0 1px #000;
  box-shadow: inset 0 0 1px #000;
  background: rgba(240, 240, 240, 0.5);
}
div.output_prompt {
  color: #D84315;
}
/* This class is the outer container of all output sections. */
div.output_area {
  padding: 0px;
  page-break-inside: avoid;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
div.output_area .MathJax_Display {
  text-align: left !important;
}
div.output_area .rendered_html table {
  margin-left: 0;
  margin-right: 0;
}
div.output_area .rendered_html img {
  margin-left: 0;
  margin-right: 0;
}
div.output_area img,
div.output_area svg {
  max-width: 100%;
  height: auto;
}
div.output_area img.unconfined,
div.output_area svg.unconfined {
  max-width: none;
}
div.output_area .mglyph > img {
  max-width: none;
}
/* This is needed to protect the pre formating from global settings such
   as that of bootstrap */
.output {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: vertical;
  -moz-box-align: stretch;
  display: box;
  box-orient: vertical;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.output_area {
    /* Old browsers */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-box-align: stretch;
    display: -moz-box;
    -moz-box-orient: vertical;
    -moz-box-align: stretch;
    display: box;
    box-orient: vertical;
    box-align: stretch;
    /* Modern browsers */
    display: flex;
    flex-direction: column;
    align-items: stretch;
  }
}
div.output_area pre {
  margin: 0;
  padding: 1px 0 1px 0;
  border: 0;
  vertical-align: baseline;
  color: black;
  background-color: transparent;
  border-radius: 0;
}
/* This class is for the output subarea inside the output_area and after
   the prompt div. */
div.output_subarea {
  overflow-x: auto;
  padding: 0.4em;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
  max-width: calc(100% - 14ex);
}
div.output_scroll div.output_subarea {
  overflow-x: visible;
}
/* The rest of the output_* classes are for special styling of the different
   output types */
/* all text output has this class: */
div.output_text {
  text-align: left;
  color: #000;
  /* This has to match that of the the CodeMirror class line-height below */
  line-height: 1.21429em;
}
/* stdout/stderr are 'text' as well as 'stream', but execute_result/error are *not* streams */
div.output_stderr {
  background: #fdd;
  /* very light red background for stderr */
}
div.output_latex {
  text-align: left;
}
/* Empty output_javascript divs should have no height */
div.output_javascript:empty {
  padding: 0;
}
.js-error {
  color: darkred;
}
/* raw_input styles */
div.raw_input_container {
  line-height: 1.21429em;
  padding-top: 5px;
}
pre.raw_input_prompt {
  /* nothing needed here. */
}
input.raw_input {
  font-family: monospace;
  font-size: inherit;
  color: inherit;
  width: auto;
  /* make sure input baseline aligns with prompt */
  vertical-align: baseline;
  /* padding + margin = 0.5em between prompt and cursor */
  padding: 0em 0.25em;
  margin: 0em 0.25em;
}
input.raw_input:focus {
  box-shadow: none;
}
p.p-space {
  margin-bottom: 10px;
}
div.output_unrecognized {
  padding: 5px;
  font-weight: bold;
  color: red;
}
div.output_unrecognized a {
  color: inherit;
  text-decoration: none;
}
div.output_unrecognized a:hover {
  color: inherit;
  text-decoration: none;
}
.rendered_html {
  color: #000;
  /* any extras will just be numbers: */
}
.rendered_html em {
  font-style: italic;
}
.rendered_html strong {
  font-weight: bold;
}
.rendered_html u {
  text-decoration: underline;
}
.rendered_html :link {
  text-decoration: underline;
}
.rendered_html :visited {
  text-decoration: underline;
}
.rendered_html h1 {
  font-size: 185.7%;
  margin: 1.08em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h2 {
  font-size: 157.1%;
  margin: 1.27em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h3 {
  font-size: 128.6%;
  margin: 1.55em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h4 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
}
.rendered_html h5 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
  font-style: italic;
}
.rendered_html h6 {
  font-size: 100%;
  margin: 2em 0 0 0;
  font-weight: bold;
  line-height: 1.0;
  font-style: italic;
}
.rendered_html h1:first-child {
  margin-top: 0.538em;
}
.rendered_html h2:first-child {
  margin-top: 0.636em;
}
.rendered_html h3:first-child {
  margin-top: 0.777em;
}
.rendered_html h4:first-child {
  margin-top: 1em;
}
.rendered_html h5:first-child {
  margin-top: 1em;
}
.rendered_html h6:first-child {
  margin-top: 1em;
}
.rendered_html ul:not(.list-inline),
.rendered_html ol:not(.list-inline) {
  padding-left: 2em;
}
.rendered_html ul {
  list-style: disc;
}
.rendered_html ul ul {
  list-style: square;
  margin-top: 0;
}
.rendered_html ul ul ul {
  list-style: circle;
}
.rendered_html ol {
  list-style: decimal;
}
.rendered_html ol ol {
  list-style: upper-alpha;
  margin-top: 0;
}
.rendered_html ol ol ol {
  list-style: lower-alpha;
}
.rendered_html ol ol ol ol {
  list-style: lower-roman;
}
.rendered_html ol ol ol ol ol {
  list-style: decimal;
}
.rendered_html * + ul {
  margin-top: 1em;
}
.rendered_html * + ol {
  margin-top: 1em;
}
.rendered_html hr {
  color: black;
  background-color: black;
}
.rendered_html pre {
  margin: 1em 2em;
  padding: 0px;
  background-color: #fff;
}
.rendered_html code {
  background-color: #eff0f1;
}
.rendered_html p code {
  padding: 1px 5px;
}
.rendered_html pre code {
  background-color: #fff;
}
.rendered_html pre,
.rendered_html code {
  border: 0;
  color: #000;
  font-size: 100%;
}
.rendered_html blockquote {
  margin: 1em 2em;
}
.rendered_html table {
  margin-left: auto;
  margin-right: auto;
  border: none;
  border-collapse: collapse;
  border-spacing: 0;
  color: black;
  font-size: 12px;
  table-layout: fixed;
}
.rendered_html thead {
  border-bottom: 1px solid black;
  vertical-align: bottom;
}
.rendered_html tr,
.rendered_html th,
.rendered_html td {
  text-align: right;
  vertical-align: middle;
  padding: 0.5em 0.5em;
  line-height: normal;
  white-space: normal;
  max-width: none;
  border: none;
}
.rendered_html th {
  font-weight: bold;
}
.rendered_html tbody tr:nth-child(odd) {
  background: #f5f5f5;
}
.rendered_html tbody tr:hover {
  background: rgba(66, 165, 245, 0.2);
}
.rendered_html * + table {
  margin-top: 1em;
}
.rendered_html p {
  text-align: left;
}
.rendered_html * + p {
  margin-top: 1em;
}
.rendered_html img {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
.rendered_html * + img {
  margin-top: 1em;
}
.rendered_html img,
.rendered_html svg {
  max-width: 100%;
  height: auto;
}
.rendered_html img.unconfined,
.rendered_html svg.unconfined {
  max-width: none;
}
.rendered_html .alert {
  margin-bottom: initial;
}
.rendered_html * + .alert {
  margin-top: 1em;
}
[dir="rtl"] .rendered_html p {
  text-align: right;
}
div.text_cell {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
}
@media (max-width: 540px) {
  div.text_cell > div.prompt {
    display: none;
  }
}
div.text_cell_render {
  /*font-family: "Helvetica Neue", Arial, Helvetica, Geneva, sans-serif;*/
  outline: none;
  resize: none;
  width: inherit;
  border-style: none;
  padding: 0.5em 0.5em 0.5em 0.4em;
  color: #000;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
a.anchor-link:link {
  text-decoration: none;
  padding: 0px 20px;
  visibility: hidden;
}
h1:hover .anchor-link,
h2:hover .anchor-link,
h3:hover .anchor-link,
h4:hover .anchor-link,
h5:hover .anchor-link,
h6:hover .anchor-link {
  visibility: visible;
}
.text_cell.rendered .input_area {
  display: none;
}
.text_cell.rendered .rendered_html {
  overflow-x: auto;
  overflow-y: hidden;
}
.text_cell.rendered .rendered_html tr,
.text_cell.rendered .rendered_html th,
.text_cell.rendered .rendered_html td {
  max-width: none;
}
.text_cell.unrendered .text_cell_render {
  display: none;
}
.text_cell .dropzone .input_area {
  border: 2px dashed #bababa;
  margin: -1px;
}
.cm-header-1,
.cm-header-2,
.cm-header-3,
.cm-header-4,
.cm-header-5,
.cm-header-6 {
  font-weight: bold;
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
}
.cm-header-1 {
  font-size: 185.7%;
}
.cm-header-2 {
  font-size: 157.1%;
}
.cm-header-3 {
  font-size: 128.6%;
}
.cm-header-4 {
  font-size: 110%;
}
.cm-header-5 {
  font-size: 100%;
  font-style: italic;
}
.cm-header-6 {
  font-size: 100%;
  font-style: italic;
}
/*!
*
* IPython notebook webapp
*
*/
@media (max-width: 767px) {
  .notebook_app {
    padding-left: 0px;
    padding-right: 0px;
  }
}
#ipython-main-app {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  height: 100%;
}
div#notebook_panel {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  height: 100%;
}
div#notebook {
  font-size: 14px;
  line-height: 20px;
  overflow-y: hidden;
  overflow-x: auto;
  width: 100%;
  /* This spaces the page away from the edge of the notebook area */
  padding-top: 20px;
  margin: 0px;
  outline: none;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  min-height: 100%;
}
@media not print {
  #notebook-container {
    padding: 15px;
    background-color: #fff;
    min-height: 0;
    -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
    box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  }
}
@media print {
  #notebook-container {
    width: 100%;
  }
}
div.ui-widget-content {
  border: 1px solid #ababab;
  outline: none;
}
pre.dialog {
  background-color: #f7f7f7;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0.4em;
  padding-left: 2em;
}
p.dialog {
  padding: 0.2em;
}
/* Word-wrap output correctly.  This is the CSS3 spelling, though Firefox seems
   to not honor it correctly.  Webkit browsers (Chrome, rekonq, Safari) do.
 */
pre,
code,
kbd,
samp {
  white-space: pre-wrap;
}
#fonttest {
  font-family: monospace;
}
p {
  margin-bottom: 0;
}
.end_space {
  min-height: 100px;
  transition: height .2s ease;
}
.notebook_app > #header {
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
@media not print {
  .notebook_app {
    background-color: #EEE;
  }
}
kbd {
  border-style: solid;
  border-width: 1px;
  box-shadow: none;
  margin: 2px;
  padding-left: 2px;
  padding-right: 2px;
  padding-top: 1px;
  padding-bottom: 1px;
}
.jupyter-keybindings {
  padding: 1px;
  line-height: 24px;
  border-bottom: 1px solid gray;
}
.jupyter-keybindings input {
  margin: 0;
  padding: 0;
  border: none;
}
.jupyter-keybindings i {
  padding: 6px;
}
.well code {
  background-color: #ffffff;
  border-color: #ababab;
  border-width: 1px;
  border-style: solid;
  padding: 2px;
  padding-top: 1px;
  padding-bottom: 1px;
}
/* CSS for the cell toolbar */
.celltoolbar {
  border: thin solid #CFCFCF;
  border-bottom: none;
  background: #EEE;
  border-radius: 2px 2px 0px 0px;
  width: 100%;
  height: 29px;
  padding-right: 4px;
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
  /* Old browsers */
  -webkit-box-pack: end;
  -moz-box-pack: end;
  box-pack: end;
  /* Modern browsers */
  justify-content: flex-end;
  display: -webkit-flex;
}
@media print {
  .celltoolbar {
    display: none;
  }
}
.ctb_hideshow {
  display: none;
  vertical-align: bottom;
}
/* ctb_show is added to the ctb_hideshow div to show the cell toolbar.
   Cell toolbars are only shown when the ctb_global_show class is also set.
*/
.ctb_global_show .ctb_show.ctb_hideshow {
  display: block;
}
.ctb_global_show .ctb_show + .input_area,
.ctb_global_show .ctb_show + div.text_cell_input,
.ctb_global_show .ctb_show ~ div.text_cell_render {
  border-top-right-radius: 0px;
  border-top-left-radius: 0px;
}
.ctb_global_show .ctb_show ~ div.text_cell_render {
  border: 1px solid #cfcfcf;
}
.celltoolbar {
  font-size: 87%;
  padding-top: 3px;
}
.celltoolbar select {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
  width: inherit;
  font-size: inherit;
  height: 22px;
  padding: 0px;
  display: inline-block;
}
.celltoolbar select:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.celltoolbar select::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.celltoolbar select:-ms-input-placeholder {
  color: #999;
}
.celltoolbar select::-webkit-input-placeholder {
  color: #999;
}
.celltoolbar select::-ms-expand {
  border: 0;
  background-color: transparent;
}
.celltoolbar select[disabled],
.celltoolbar select[readonly],
fieldset[disabled] .celltoolbar select {
  background-color: #eeeeee;
  opacity: 1;
}
.celltoolbar select[disabled],
fieldset[disabled] .celltoolbar select {
  cursor: not-allowed;
}
textarea.celltoolbar select {
  height: auto;
}
select.celltoolbar select {
  height: 30px;
  line-height: 30px;
}
textarea.celltoolbar select,
select[multiple].celltoolbar select {
  height: auto;
}
.celltoolbar label {
  margin-left: 5px;
  margin-right: 5px;
}
.tags_button_container {
  width: 100%;
  display: flex;
}
.tag-container {
  display: flex;
  flex-direction: row;
  flex-grow: 1;
  overflow: hidden;
  position: relative;
}
.tag-container > * {
  margin: 0 4px;
}
.remove-tag-btn {
  margin-left: 4px;
}
.tags-input {
  display: flex;
}
.cell-tag:last-child:after {
  content: "";
  position: absolute;
  right: 0;
  width: 40px;
  height: 100%;
  /* Fade to background color of cell toolbar */
  background: linear-gradient(to right, rgba(0, 0, 0, 0), #EEE);
}
.tags-input > * {
  margin-left: 4px;
}
.cell-tag,
.tags-input input,
.tags-input button {
  display: block;
  width: 100%;
  height: 32px;
  padding: 6px 12px;
  font-size: 13px;
  line-height: 1.42857143;
  color: #555555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 2px;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075);
  -webkit-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  -o-transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s;
  height: 30px;
  padding: 5px 10px;
  font-size: 12px;
  line-height: 1.5;
  border-radius: 1px;
  box-shadow: none;
  width: inherit;
  font-size: inherit;
  height: 22px;
  line-height: 22px;
  padding: 0px 4px;
  display: inline-block;
}
.cell-tag:focus,
.tags-input input:focus,
.tags-input button:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102, 175, 233, 0.6);
}
.cell-tag::-moz-placeholder,
.tags-input input::-moz-placeholder,
.tags-input button::-moz-placeholder {
  color: #999;
  opacity: 1;
}
.cell-tag:-ms-input-placeholder,
.tags-input input:-ms-input-placeholder,
.tags-input button:-ms-input-placeholder {
  color: #999;
}
.cell-tag::-webkit-input-placeholder,
.tags-input input::-webkit-input-placeholder,
.tags-input button::-webkit-input-placeholder {
  color: #999;
}
.cell-tag::-ms-expand,
.tags-input input::-ms-expand,
.tags-input button::-ms-expand {
  border: 0;
  background-color: transparent;
}
.cell-tag[disabled],
.tags-input input[disabled],
.tags-input button[disabled],
.cell-tag[readonly],
.tags-input input[readonly],
.tags-input button[readonly],
fieldset[disabled] .cell-tag,
fieldset[disabled] .tags-input input,
fieldset[disabled] .tags-input button {
  background-color: #eeeeee;
  opacity: 1;
}
.cell-tag[disabled],
.tags-input input[disabled],
.tags-input button[disabled],
fieldset[disabled] .cell-tag,
fieldset[disabled] .tags-input input,
fieldset[disabled] .tags-input button {
  cursor: not-allowed;
}
textarea.cell-tag,
textarea.tags-input input,
textarea.tags-input button {
  height: auto;
}
select.cell-tag,
select.tags-input input,
select.tags-input button {
  height: 30px;
  line-height: 30px;
}
textarea.cell-tag,
textarea.tags-input input,
textarea.tags-input button,
select[multiple].cell-tag,
select[multiple].tags-input input,
select[multiple].tags-input button {
  height: auto;
}
.cell-tag,
.tags-input button {
  padding: 0px 4px;
}
.cell-tag {
  background-color: #fff;
  white-space: nowrap;
}
.tags-input input[type=text]:focus {
  outline: none;
  box-shadow: none;
  border-color: #ccc;
}
.completions {
  position: absolute;
  z-index: 110;
  overflow: hidden;
  border: 1px solid #ababab;
  border-radius: 2px;
  -webkit-box-shadow: 0px 6px 10px -1px #adadad;
  box-shadow: 0px 6px 10px -1px #adadad;
  line-height: 1;
}
.completions select {
  background: white;
  outline: none;
  border: none;
  padding: 0px;
  margin: 0px;
  overflow: auto;
  font-family: monospace;
  font-size: 110%;
  color: #000;
  width: auto;
}
.completions select option.context {
  color: #286090;
}
#kernel_logo_widget .current_kernel_logo {
  display: none;
  margin-top: -1px;
  margin-bottom: -1px;
  width: 32px;
  height: 32px;
}
[dir="rtl"] #kernel_logo_widget {
  float: left !important;
  float: left;
}
.modal .modal-body .move-path {
  display: flex;
  flex-direction: row;
  justify-content: space;
  align-items: center;
}
.modal .modal-body .move-path .server-root {
  padding-right: 20px;
}
.modal .modal-body .move-path .path-input {
  flex: 1;
}
#menubar {
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  margin-top: 1px;
}
#menubar .navbar {
  border-top: 1px;
  border-radius: 0px 0px 2px 2px;
  margin-bottom: 0px;
}
#menubar .navbar-toggle {
  float: left;
  padding-top: 7px;
  padding-bottom: 7px;
  border: none;
}
#menubar .navbar-collapse {
  clear: left;
}
[dir="rtl"] #menubar .navbar-toggle {
  float: right;
}
[dir="rtl"] #menubar .navbar-collapse {
  clear: right;
}
[dir="rtl"] #menubar .navbar-nav {
  float: right;
}
[dir="rtl"] #menubar .nav {
  padding-right: 0px;
}
[dir="rtl"] #menubar .navbar-nav > li {
  float: right;
}
[dir="rtl"] #menubar .navbar-right {
  float: left !important;
}
[dir="rtl"] ul.dropdown-menu {
  text-align: right;
  left: auto;
}
[dir="rtl"] ul#new-menu.dropdown-menu {
  right: auto;
  left: 0;
}
.nav-wrapper {
  border-bottom: 1px solid #e7e7e7;
}
i.menu-icon {
  padding-top: 4px;
}
[dir="rtl"] i.menu-icon.pull-right {
  float: left !important;
  float: left;
}
ul#help_menu li a {
  overflow: hidden;
  padding-right: 2.2em;
}
ul#help_menu li a i {
  margin-right: -1.2em;
}
[dir="rtl"] ul#help_menu li a {
  padding-left: 2.2em;
}
[dir="rtl"] ul#help_menu li a i {
  margin-right: 0;
  margin-left: -1.2em;
}
[dir="rtl"] ul#help_menu li a i.pull-right {
  float: left !important;
  float: left;
}
.dropdown-submenu {
  position: relative;
}
.dropdown-submenu > .dropdown-menu {
  top: 0;
  left: 100%;
  margin-top: -6px;
  margin-left: -1px;
}
[dir="rtl"] .dropdown-submenu > .dropdown-menu {
  right: 100%;
  margin-right: -1px;
}
.dropdown-submenu:hover > .dropdown-menu {
  display: block;
}
.dropdown-submenu > a:after {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: block;
  content: "\f0da";
  float: right;
  color: #333333;
  margin-top: 2px;
  margin-right: -10px;
}
.dropdown-submenu > a:after.fa-pull-left {
  margin-right: .3em;
}
.dropdown-submenu > a:after.fa-pull-right {
  margin-left: .3em;
}
.dropdown-submenu > a:after.pull-left {
  margin-right: .3em;
}
.dropdown-submenu > a:after.pull-right {
  margin-left: .3em;
}
[dir="rtl"] .dropdown-submenu > a:after {
  float: left;
  content: "\f0d9";
  margin-right: 0;
  margin-left: -10px;
}
.dropdown-submenu:hover > a:after {
  color: #262626;
}
.dropdown-submenu.pull-left {
  float: none;
}
.dropdown-submenu.pull-left > .dropdown-menu {
  left: -100%;
  margin-left: 10px;
}
#notification_area {
  float: right !important;
  float: right;
  z-index: 10;
}
[dir="rtl"] #notification_area {
  float: left !important;
  float: left;
}
.indicator_area {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
}
[dir="rtl"] .indicator_area {
  float: left !important;
  float: left;
}
#kernel_indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
  border-left: 1px solid;
}
#kernel_indicator .kernel_indicator_name {
  padding-left: 5px;
  padding-right: 5px;
}
[dir="rtl"] #kernel_indicator {
  float: left !important;
  float: left;
  border-left: 0;
  border-right: 1px solid;
}
#modal_indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
}
[dir="rtl"] #modal_indicator {
  float: left !important;
  float: left;
}
#readonly-indicator {
  float: right !important;
  float: right;
  color: #777;
  margin-left: 5px;
  margin-right: 5px;
  width: 11px;
  z-index: 10;
  text-align: center;
  width: auto;
  margin-top: 2px;
  margin-bottom: 0px;
  margin-left: 0px;
  margin-right: 0px;
  display: none;
}
.modal_indicator:before {
  width: 1.28571429em;
  text-align: center;
}
.edit_mode .modal_indicator:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f040";
}
.edit_mode .modal_indicator:before.fa-pull-left {
  margin-right: .3em;
}
.edit_mode .modal_indicator:before.fa-pull-right {
  margin-left: .3em;
}
.edit_mode .modal_indicator:before.pull-left {
  margin-right: .3em;
}
.edit_mode .modal_indicator:before.pull-right {
  margin-left: .3em;
}
.command_mode .modal_indicator:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: ' ';
}
.command_mode .modal_indicator:before.fa-pull-left {
  margin-right: .3em;
}
.command_mode .modal_indicator:before.fa-pull-right {
  margin-left: .3em;
}
.command_mode .modal_indicator:before.pull-left {
  margin-right: .3em;
}
.command_mode .modal_indicator:before.pull-right {
  margin-left: .3em;
}
.kernel_idle_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f10c";
}
.kernel_idle_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_idle_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_idle_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_idle_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_busy_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f111";
}
.kernel_busy_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_busy_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_busy_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_busy_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_dead_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f1e2";
}
.kernel_dead_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_dead_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_dead_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_dead_icon:before.pull-right {
  margin-left: .3em;
}
.kernel_disconnected_icon:before {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: inherit;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  content: "\f127";
}
.kernel_disconnected_icon:before.fa-pull-left {
  margin-right: .3em;
}
.kernel_disconnected_icon:before.fa-pull-right {
  margin-left: .3em;
}
.kernel_disconnected_icon:before.pull-left {
  margin-right: .3em;
}
.kernel_disconnected_icon:before.pull-right {
  margin-left: .3em;
}
.notification_widget {
  color: #777;
  z-index: 10;
  background: rgba(240, 240, 240, 0.5);
  margin-right: 4px;
  color: #333;
  background-color: #fff;
  border-color: #ccc;
}
.notification_widget:focus,
.notification_widget.focus {
  color: #333;
  background-color: #e6e6e6;
  border-color: #8c8c8c;
}
.notification_widget:hover {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.notification_widget:active,
.notification_widget.active,
.open > .dropdown-toggle.notification_widget {
  color: #333;
  background-color: #e6e6e6;
  border-color: #adadad;
}
.notification_widget:active:hover,
.notification_widget.active:hover,
.open > .dropdown-toggle.notification_widget:hover,
.notification_widget:active:focus,
.notification_widget.active:focus,
.open > .dropdown-toggle.notification_widget:focus,
.notification_widget:active.focus,
.notification_widget.active.focus,
.open > .dropdown-toggle.notification_widget.focus {
  color: #333;
  background-color: #d4d4d4;
  border-color: #8c8c8c;
}
.notification_widget:active,
.notification_widget.active,
.open > .dropdown-toggle.notification_widget {
  background-image: none;
}
.notification_widget.disabled:hover,
.notification_widget[disabled]:hover,
fieldset[disabled] .notification_widget:hover,
.notification_widget.disabled:focus,
.notification_widget[disabled]:focus,
fieldset[disabled] .notification_widget:focus,
.notification_widget.disabled.focus,
.notification_widget[disabled].focus,
fieldset[disabled] .notification_widget.focus {
  background-color: #fff;
  border-color: #ccc;
}
.notification_widget .badge {
  color: #fff;
  background-color: #333;
}
.notification_widget.warning {
  color: #fff;
  background-color: #f0ad4e;
  border-color: #eea236;
}
.notification_widget.warning:focus,
.notification_widget.warning.focus {
  color: #fff;
  background-color: #ec971f;
  border-color: #985f0d;
}
.notification_widget.warning:hover {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.notification_widget.warning:active,
.notification_widget.warning.active,
.open > .dropdown-toggle.notification_widget.warning {
  color: #fff;
  background-color: #ec971f;
  border-color: #d58512;
}
.notification_widget.warning:active:hover,
.notification_widget.warning.active:hover,
.open > .dropdown-toggle.notification_widget.warning:hover,
.notification_widget.warning:active:focus,
.notification_widget.warning.active:focus,
.open > .dropdown-toggle.notification_widget.warning:focus,
.notification_widget.warning:active.focus,
.notification_widget.warning.active.focus,
.open > .dropdown-toggle.notification_widget.warning.focus {
  color: #fff;
  background-color: #d58512;
  border-color: #985f0d;
}
.notification_widget.warning:active,
.notification_widget.warning.active,
.open > .dropdown-toggle.notification_widget.warning {
  background-image: none;
}
.notification_widget.warning.disabled:hover,
.notification_widget.warning[disabled]:hover,
fieldset[disabled] .notification_widget.warning:hover,
.notification_widget.warning.disabled:focus,
.notification_widget.warning[disabled]:focus,
fieldset[disabled] .notification_widget.warning:focus,
.notification_widget.warning.disabled.focus,
.notification_widget.warning[disabled].focus,
fieldset[disabled] .notification_widget.warning.focus {
  background-color: #f0ad4e;
  border-color: #eea236;
}
.notification_widget.warning .badge {
  color: #f0ad4e;
  background-color: #fff;
}
.notification_widget.success {
  color: #fff;
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.notification_widget.success:focus,
.notification_widget.success.focus {
  color: #fff;
  background-color: #449d44;
  border-color: #255625;
}
.notification_widget.success:hover {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.notification_widget.success:active,
.notification_widget.success.active,
.open > .dropdown-toggle.notification_widget.success {
  color: #fff;
  background-color: #449d44;
  border-color: #398439;
}
.notification_widget.success:active:hover,
.notification_widget.success.active:hover,
.open > .dropdown-toggle.notification_widget.success:hover,
.notification_widget.success:active:focus,
.notification_widget.success.active:focus,
.open > .dropdown-toggle.notification_widget.success:focus,
.notification_widget.success:active.focus,
.notification_widget.success.active.focus,
.open > .dropdown-toggle.notification_widget.success.focus {
  color: #fff;
  background-color: #398439;
  border-color: #255625;
}
.notification_widget.success:active,
.notification_widget.success.active,
.open > .dropdown-toggle.notification_widget.success {
  background-image: none;
}
.notification_widget.success.disabled:hover,
.notification_widget.success[disabled]:hover,
fieldset[disabled] .notification_widget.success:hover,
.notification_widget.success.disabled:focus,
.notification_widget.success[disabled]:focus,
fieldset[disabled] .notification_widget.success:focus,
.notification_widget.success.disabled.focus,
.notification_widget.success[disabled].focus,
fieldset[disabled] .notification_widget.success.focus {
  background-color: #5cb85c;
  border-color: #4cae4c;
}
.notification_widget.success .badge {
  color: #5cb85c;
  background-color: #fff;
}
.notification_widget.info {
  color: #fff;
  background-color: #5bc0de;
  border-color: #46b8da;
}
.notification_widget.info:focus,
.notification_widget.info.focus {
  color: #fff;
  background-color: #31b0d5;
  border-color: #1b6d85;
}
.notification_widget.info:hover {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.notification_widget.info:active,
.notification_widget.info.active,
.open > .dropdown-toggle.notification_widget.info {
  color: #fff;
  background-color: #31b0d5;
  border-color: #269abc;
}
.notification_widget.info:active:hover,
.notification_widget.info.active:hover,
.open > .dropdown-toggle.notification_widget.info:hover,
.notification_widget.info:active:focus,
.notification_widget.info.active:focus,
.open > .dropdown-toggle.notification_widget.info:focus,
.notification_widget.info:active.focus,
.notification_widget.info.active.focus,
.open > .dropdown-toggle.notification_widget.info.focus {
  color: #fff;
  background-color: #269abc;
  border-color: #1b6d85;
}
.notification_widget.info:active,
.notification_widget.info.active,
.open > .dropdown-toggle.notification_widget.info {
  background-image: none;
}
.notification_widget.info.disabled:hover,
.notification_widget.info[disabled]:hover,
fieldset[disabled] .notification_widget.info:hover,
.notification_widget.info.disabled:focus,
.notification_widget.info[disabled]:focus,
fieldset[disabled] .notification_widget.info:focus,
.notification_widget.info.disabled.focus,
.notification_widget.info[disabled].focus,
fieldset[disabled] .notification_widget.info.focus {
  background-color: #5bc0de;
  border-color: #46b8da;
}
.notification_widget.info .badge {
  color: #5bc0de;
  background-color: #fff;
}
.notification_widget.danger {
  color: #fff;
  background-color: #d9534f;
  border-color: #d43f3a;
}
.notification_widget.danger:focus,
.notification_widget.danger.focus {
  color: #fff;
  background-color: #c9302c;
  border-color: #761c19;
}
.notification_widget.danger:hover {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.notification_widget.danger:active,
.notification_widget.danger.active,
.open > .dropdown-toggle.notification_widget.danger {
  color: #fff;
  background-color: #c9302c;
  border-color: #ac2925;
}
.notification_widget.danger:active:hover,
.notification_widget.danger.active:hover,
.open > .dropdown-toggle.notification_widget.danger:hover,
.notification_widget.danger:active:focus,
.notification_widget.danger.active:focus,
.open > .dropdown-toggle.notification_widget.danger:focus,
.notification_widget.danger:active.focus,
.notification_widget.danger.active.focus,
.open > .dropdown-toggle.notification_widget.danger.focus {
  color: #fff;
  background-color: #ac2925;
  border-color: #761c19;
}
.notification_widget.danger:active,
.notification_widget.danger.active,
.open > .dropdown-toggle.notification_widget.danger {
  background-image: none;
}
.notification_widget.danger.disabled:hover,
.notification_widget.danger[disabled]:hover,
fieldset[disabled] .notification_widget.danger:hover,
.notification_widget.danger.disabled:focus,
.notification_widget.danger[disabled]:focus,
fieldset[disabled] .notification_widget.danger:focus,
.notification_widget.danger.disabled.focus,
.notification_widget.danger[disabled].focus,
fieldset[disabled] .notification_widget.danger.focus {
  background-color: #d9534f;
  border-color: #d43f3a;
}
.notification_widget.danger .badge {
  color: #d9534f;
  background-color: #fff;
}
div#pager {
  background-color: #fff;
  font-size: 14px;
  line-height: 20px;
  overflow: hidden;
  display: none;
  position: fixed;
  bottom: 0px;
  width: 100%;
  max-height: 50%;
  padding-top: 8px;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  /* Display over codemirror */
  z-index: 100;
  /* Hack which prevents jquery ui resizable from changing top. */
  top: auto !important;
}
div#pager pre {
  line-height: 1.21429em;
  color: #000;
  background-color: #f7f7f7;
  padding: 0.4em;
}
div#pager #pager-button-area {
  position: absolute;
  top: 8px;
  right: 20px;
}
div#pager #pager-contents {
  position: relative;
  overflow: auto;
  width: 100%;
  height: 100%;
}
div#pager #pager-contents #pager-container {
  position: relative;
  padding: 15px 0px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
div#pager .ui-resizable-handle {
  top: 0px;
  height: 8px;
  background: #f7f7f7;
  border-top: 1px solid #cfcfcf;
  border-bottom: 1px solid #cfcfcf;
  /* This injects handle bars (a short, wide = symbol) for 
        the resize handle. */
}
div#pager .ui-resizable-handle::after {
  content: '';
  top: 2px;
  left: 50%;
  height: 3px;
  width: 30px;
  margin-left: -15px;
  position: absolute;
  border-top: 1px solid #cfcfcf;
}
.quickhelp {
  /* Old browsers */
  display: -webkit-box;
  -webkit-box-orient: horizontal;
  -webkit-box-align: stretch;
  display: -moz-box;
  -moz-box-orient: horizontal;
  -moz-box-align: stretch;
  display: box;
  box-orient: horizontal;
  box-align: stretch;
  /* Modern browsers */
  display: flex;
  flex-direction: row;
  align-items: stretch;
  line-height: 1.8em;
}
.shortcut_key {
  display: inline-block;
  width: 21ex;
  text-align: right;
  font-family: monospace;
}
.shortcut_descr {
  display: inline-block;
  /* Old browsers */
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  box-flex: 1;
  /* Modern browsers */
  flex: 1;
}
span.save_widget {
  height: 30px;
  margin-top: 4px;
  display: flex;
  justify-content: flex-start;
  align-items: baseline;
  width: 50%;
  flex: 1;
}
span.save_widget span.filename {
  height: 100%;
  line-height: 1em;
  margin-left: 16px;
  border: none;
  font-size: 146.5%;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
  border-radius: 2px;
}
span.save_widget span.filename:hover {
  background-color: #e6e6e6;
}
[dir="rtl"] span.save_widget.pull-left {
  float: right !important;
  float: right;
}
[dir="rtl"] span.save_widget span.filename {
  margin-left: 0;
  margin-right: 16px;
}
span.checkpoint_status,
span.autosave_status {
  font-size: small;
  white-space: nowrap;
  padding: 0 5px;
}
@media (max-width: 767px) {
  span.save_widget {
    font-size: small;
    padding: 0 0 0 5px;
  }
  span.checkpoint_status,
  span.autosave_status {
    display: none;
  }
}
@media (min-width: 768px) and (max-width: 991px) {
  span.checkpoint_status {
    display: none;
  }
  span.autosave_status {
    font-size: x-small;
  }
}
.toolbar {
  padding: 0px;
  margin-left: -5px;
  margin-top: 2px;
  margin-bottom: 5px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
}
.toolbar select,
.toolbar label {
  width: auto;
  vertical-align: middle;
  margin-right: 2px;
  margin-bottom: 0px;
  display: inline;
  font-size: 92%;
  margin-left: 0.3em;
  margin-right: 0.3em;
  padding: 0px;
  padding-top: 3px;
}
.toolbar .btn {
  padding: 2px 8px;
}
.toolbar .btn-group {
  margin-top: 0px;
  margin-left: 5px;
}
.toolbar-btn-label {
  margin-left: 6px;
}
#maintoolbar {
  margin-bottom: -3px;
  margin-top: -8px;
  border: 0px;
  min-height: 27px;
  margin-left: 0px;
  padding-top: 11px;
  padding-bottom: 3px;
}
#maintoolbar .navbar-text {
  float: none;
  vertical-align: middle;
  text-align: right;
  margin-left: 5px;
  margin-right: 0px;
  margin-top: 0px;
}
.select-xs {
  height: 24px;
}
[dir="rtl"] .btn-group > .btn,
.btn-group-vertical > .btn {
  float: right;
}
.pulse,
.dropdown-menu > li > a.pulse,
li.pulse > a.dropdown-toggle,
li.pulse.open > a.dropdown-toggle {
  background-color: #F37626;
  color: white;
}
/**
 * Primary styles
 *
 * Author: Jupyter Development Team
 */
/** WARNING IF YOU ARE EDITTING THIS FILE, if this is a .css file, It has a lot
 * of chance of beeing generated from the ../less/[samename].less file, you can
 * try to get back the less file by reverting somme commit in history
 **/
/*
 * We'll try to get something pretty, so we
 * have some strange css to have the scroll bar on
 * the left with fix button on the top right of the tooltip
 */
@-moz-keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
@-webkit-keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
@-moz-keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@-webkit-keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
/*properties of tooltip after "expand"*/
.bigtooltip {
  overflow: auto;
  height: 200px;
  -webkit-transition-property: height;
  -webkit-transition-duration: 500ms;
  -moz-transition-property: height;
  -moz-transition-duration: 500ms;
  transition-property: height;
  transition-duration: 500ms;
}
/*properties of tooltip before "expand"*/
.smalltooltip {
  -webkit-transition-property: height;
  -webkit-transition-duration: 500ms;
  -moz-transition-property: height;
  -moz-transition-duration: 500ms;
  transition-property: height;
  transition-duration: 500ms;
  text-overflow: ellipsis;
  overflow: hidden;
  height: 80px;
}
.tooltipbuttons {
  position: absolute;
  padding-right: 15px;
  top: 0px;
  right: 0px;
}
.tooltiptext {
  /*avoid the button to overlap on some docstring*/
  padding-right: 30px;
}
.ipython_tooltip {
  max-width: 700px;
  /*fade-in animation when inserted*/
  -webkit-animation: fadeOut 400ms;
  -moz-animation: fadeOut 400ms;
  animation: fadeOut 400ms;
  -webkit-animation: fadeIn 400ms;
  -moz-animation: fadeIn 400ms;
  animation: fadeIn 400ms;
  vertical-align: middle;
  background-color: #f7f7f7;
  overflow: visible;
  border: #ababab 1px solid;
  outline: none;
  padding: 3px;
  margin: 0px;
  padding-left: 7px;
  font-family: monospace;
  min-height: 50px;
  -moz-box-shadow: 0px 6px 10px -1px #adadad;
  -webkit-box-shadow: 0px 6px 10px -1px #adadad;
  box-shadow: 0px 6px 10px -1px #adadad;
  border-radius: 2px;
  position: absolute;
  z-index: 1000;
}
.ipython_tooltip a {
  float: right;
}
.ipython_tooltip .tooltiptext pre {
  border: 0;
  border-radius: 0;
  font-size: 100%;
  background-color: #f7f7f7;
}
.pretooltiparrow {
  left: 0px;
  margin: 0px;
  top: -16px;
  width: 40px;
  height: 16px;
  overflow: hidden;
  position: absolute;
}
.pretooltiparrow:before {
  background-color: #f7f7f7;
  border: 1px #ababab solid;
  z-index: 11;
  content: "";
  position: absolute;
  left: 15px;
  top: 10px;
  width: 25px;
  height: 25px;
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
}
ul.typeahead-list i {
  margin-left: -10px;
  width: 18px;
}
[dir="rtl"] ul.typeahead-list i {
  margin-left: 0;
  margin-right: -10px;
}
ul.typeahead-list {
  max-height: 80vh;
  overflow: auto;
}
ul.typeahead-list > li > a {
  /** Firefox bug **/
  /* see https://github.com/jupyter/notebook/issues/559 */
  white-space: normal;
}
ul.typeahead-list  > li > a.pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .typeahead-list {
  text-align: right;
}
.cmd-palette .modal-body {
  padding: 7px;
}
.cmd-palette form {
  background: white;
}
.cmd-palette input {
  outline: none;
}
.no-shortcut {
  min-width: 20px;
  color: transparent;
}
[dir="rtl"] .no-shortcut.pull-right {
  float: left !important;
  float: left;
}
[dir="rtl"] .command-shortcut.pull-right {
  float: left !important;
  float: left;
}
.command-shortcut:before {
  content: "(command mode)";
  padding-right: 3px;
  color: #777777;
}
.edit-shortcut:before {
  content: "(edit)";
  padding-right: 3px;
  color: #777777;
}
[dir="rtl"] .edit-shortcut.pull-right {
  float: left !important;
  float: left;
}
#find-and-replace #replace-preview .match,
#find-and-replace #replace-preview .insert {
  background-color: #BBDEFB;
  border-color: #90CAF9;
  border-style: solid;
  border-width: 1px;
  border-radius: 0px;
}
[dir="ltr"] #find-and-replace .input-group-btn + .form-control {
  border-left: none;
}
[dir="rtl"] #find-and-replace .input-group-btn + .form-control {
  border-right: none;
}
#find-and-replace #replace-preview .replace .match {
  background-color: #FFCDD2;
  border-color: #EF9A9A;
  border-radius: 0px;
}
#find-and-replace #replace-preview .replace .insert {
  background-color: #C8E6C9;
  border-color: #A5D6A7;
  border-radius: 0px;
}
#find-and-replace #replace-preview {
  max-height: 60vh;
  overflow: auto;
}
#find-and-replace #replace-preview pre {
  padding: 5px 10px;
}
.terminal-app {
  background: #EEE;
}
.terminal-app #header {
  background: #fff;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.2);
}
.terminal-app .terminal {
  width: 100%;
  float: left;
  font-family: monospace;
  color: white;
  background: black;
  padding: 0.4em;
  border-radius: 2px;
  -webkit-box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.4);
  box-shadow: 0px 0px 12px 1px rgba(87, 87, 87, 0.4);
}
.terminal-app .terminal,
.terminal-app .terminal dummy-screen {
  line-height: 1em;
  font-size: 14px;
}
.terminal-app .terminal .xterm-rows {
  padding: 10px;
}
.terminal-app .terminal-cursor {
  color: black;
  background: white;
}
.terminal-app #terminado-container {
  margin-top: 20px;
}
/*# sourceMappingURL=style.min.css.map */
    </style>
<style type="text/css">
    .highlight .hll { background-color: #ffffcc }
.highlight  { background: #f8f8f8; }
.highlight .c { color: #408080; font-style: italic } /* Comment */
.highlight .err { border: 1px solid #FF0000 } /* Error */
.highlight .k { color: #008000; font-weight: bold } /* Keyword */
.highlight .o { color: #666666 } /* Operator */
.highlight .ch { color: #408080; font-style: italic } /* Comment.Hashbang */
.highlight .cm { color: #408080; font-style: italic } /* Comment.Multiline */
.highlight .cp { color: #BC7A00 } /* Comment.Preproc */
.highlight .cpf { color: #408080; font-style: italic } /* Comment.PreprocFile */
.highlight .c1 { color: #408080; font-style: italic } /* Comment.Single */
.highlight .cs { color: #408080; font-style: italic } /* Comment.Special */
.highlight .gd { color: #A00000 } /* Generic.Deleted */
.highlight .ge { font-style: italic } /* Generic.Emph */
.highlight .gr { color: #FF0000 } /* Generic.Error */
.highlight .gh { color: #000080; font-weight: bold } /* Generic.Heading */
.highlight .gi { color: #00A000 } /* Generic.Inserted */
.highlight .go { color: #888888 } /* Generic.Output */
.highlight .gp { color: #000080; font-weight: bold } /* Generic.Prompt */
.highlight .gs { font-weight: bold } /* Generic.Strong */
.highlight .gu { color: #800080; font-weight: bold } /* Generic.Subheading */
.highlight .gt { color: #0044DD } /* Generic.Traceback */
.highlight .kc { color: #008000; font-weight: bold } /* Keyword.Constant */
.highlight .kd { color: #008000; font-weight: bold } /* Keyword.Declaration */
.highlight .kn { color: #008000; font-weight: bold } /* Keyword.Namespace */
.highlight .kp { color: #008000 } /* Keyword.Pseudo */
.highlight .kr { color: #008000; font-weight: bold } /* Keyword.Reserved */
.highlight .kt { color: #B00040 } /* Keyword.Type */
.highlight .m { color: #666666 } /* Literal.Number */
.highlight .s { color: #BA2121 } /* Literal.String */
.highlight .na { color: #7D9029 } /* Name.Attribute */
.highlight .nb { color: #008000 } /* Name.Builtin */
.highlight .nc { color: #0000FF; font-weight: bold } /* Name.Class */
.highlight .no { color: #880000 } /* Name.Constant */
.highlight .nd { color: #AA22FF } /* Name.Decorator */
.highlight .ni { color: #999999; font-weight: bold } /* Name.Entity */
.highlight .ne { color: #D2413A; font-weight: bold } /* Name.Exception */
.highlight .nf { color: #0000FF } /* Name.Function */
.highlight .nl { color: #A0A000 } /* Name.Label */
.highlight .nn { color: #0000FF; font-weight: bold } /* Name.Namespace */
.highlight .nt { color: #008000; font-weight: bold } /* Name.Tag */
.highlight .nv { color: #19177C } /* Name.Variable */
.highlight .ow { color: #AA22FF; font-weight: bold } /* Operator.Word */
.highlight .w { color: #bbbbbb } /* Text.Whitespace */
.highlight .mb { color: #666666 } /* Literal.Number.Bin */
.highlight .mf { color: #666666 } /* Literal.Number.Float */
.highlight .mh { color: #666666 } /* Literal.Number.Hex */
.highlight .mi { color: #666666 } /* Literal.Number.Integer */
.highlight .mo { color: #666666 } /* Literal.Number.Oct */
.highlight .sa { color: #BA2121 } /* Literal.String.Affix */
.highlight .sb { color: #BA2121 } /* Literal.String.Backtick */
.highlight .sc { color: #BA2121 } /* Literal.String.Char */
.highlight .dl { color: #BA2121 } /* Literal.String.Delimiter */
.highlight .sd { color: #BA2121; font-style: italic } /* Literal.String.Doc */
.highlight .s2 { color: #BA2121 } /* Literal.String.Double */
.highlight .se { color: #BB6622; font-weight: bold } /* Literal.String.Escape */
.highlight .sh { color: #BA2121 } /* Literal.String.Heredoc */
.highlight .si { color: #BB6688; font-weight: bold } /* Literal.String.Interpol */
.highlight .sx { color: #008000 } /* Literal.String.Other */
.highlight .sr { color: #BB6688 } /* Literal.String.Regex */
.highlight .s1 { color: #BA2121 } /* Literal.String.Single */
.highlight .ss { color: #19177C } /* Literal.String.Symbol */
.highlight .bp { color: #008000 } /* Name.Builtin.Pseudo */
.highlight .fm { color: #0000FF } /* Name.Function.Magic */
.highlight .vc { color: #19177C } /* Name.Variable.Class */
.highlight .vg { color: #19177C } /* Name.Variable.Global */
.highlight .vi { color: #19177C } /* Name.Variable.Instance */
.highlight .vm { color: #19177C } /* Name.Variable.Magic */
.highlight .il { color: #666666 } /* Literal.Number.Integer.Long */
    </style>
<style type="text/css">
    
/* Temporary definitions which will become obsolete with Notebook release 5.0 */
.ansi-black-fg { color: #3E424D; }
.ansi-black-bg { background-color: #3E424D; }
.ansi-black-intense-fg { color: #282C36; }
.ansi-black-intense-bg { background-color: #282C36; }
.ansi-red-fg { color: #E75C58; }
.ansi-red-bg { background-color: #E75C58; }
.ansi-red-intense-fg { color: #B22B31; }
.ansi-red-intense-bg { background-color: #B22B31; }
.ansi-green-fg { color: #00A250; }
.ansi-green-bg { background-color: #00A250; }
.ansi-green-intense-fg { color: #007427; }
.ansi-green-intense-bg { background-color: #007427; }
.ansi-yellow-fg { color: #DDB62B; }
.ansi-yellow-bg { background-color: #DDB62B; }
.ansi-yellow-intense-fg { color: #B27D12; }
.ansi-yellow-intense-bg { background-color: #B27D12; }
.ansi-blue-fg { color: #208FFB; }
.ansi-blue-bg { background-color: #208FFB; }
.ansi-blue-intense-fg { color: #0065CA; }
.ansi-blue-intense-bg { background-color: #0065CA; }
.ansi-magenta-fg { color: #D160C4; }
.ansi-magenta-bg { background-color: #D160C4; }
.ansi-magenta-intense-fg { color: #A03196; }
.ansi-magenta-intense-bg { background-color: #A03196; }
.ansi-cyan-fg { color: #60C6C8; }
.ansi-cyan-bg { background-color: #60C6C8; }
.ansi-cyan-intense-fg { color: #258F8F; }
.ansi-cyan-intense-bg { background-color: #258F8F; }
.ansi-white-fg { color: #C5C1B4; }
.ansi-white-bg { background-color: #C5C1B4; }
.ansi-white-intense-fg { color: #A1A6B2; }
.ansi-white-intense-bg { background-color: #A1A6B2; }

.ansi-bold { font-weight: bold; }

    </style>


<style type="text/css">
/* Overrides of notebook CSS for static HTML export */
body {
  overflow: visible;
  padding: 8px;
}

div#notebook {
  overflow: visible;
  border-top: none;
}@media print {
  div.cell {
    display: block;
    page-break-inside: avoid;
  } 
  div.output_wrapper { 
    display: block;
    page-break-inside: avoid; 
  }
  div.output { 
    display: block;
    page-break-inside: avoid; 
  }
}
</style>

<!-- Custom stylesheet, it must be in the same directory as the html file -->
<link rel="stylesheet" href="custom.css">

<!-- Loading mathjax macro -->
<!-- Load mathjax -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS_HTML"></script>
    <!-- MathJax configuration -->
    <script type="text/x-mathjax-config;executed=true">
    MathJax.Hub.Config({
        tex2jax: {
            inlineMath: [ ['$','$'], ["\\(","\\)"] ],
            displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
            processEscapes: true,
            processEnvironments: true
        },
        // Center justify equations in code and markdown cells. Elsewhere
        // we use CSS to left justify single line equations in code cells.
        displayAlign: 'center',
        "HTML-CSS": {
            styles: {'.MathJax_Display': {"margin": 0}},
            linebreaks: { automatic: true }
        }
    });
    </script>
    <!-- End of mathjax configuration --><script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="plotly" src="https://cdn.plot.ly/plotly-latest.min.js"></script><style type="text/css">.MathJax_Hover_Frame {border-radius: .25em; -webkit-border-radius: .25em; -moz-border-radius: .25em; -khtml-border-radius: .25em; box-shadow: 0px 0px 15px #83A; -webkit-box-shadow: 0px 0px 15px #83A; -moz-box-shadow: 0px 0px 15px #83A; -khtml-box-shadow: 0px 0px 15px #83A; border: 1px solid #A6D ! important; display: inline-block; position: absolute}
.MathJax_Menu_Button .MathJax_Hover_Arrow {position: absolute; cursor: pointer; display: inline-block; border: 2px solid #AAA; border-radius: 4px; -webkit-border-radius: 4px; -moz-border-radius: 4px; -khtml-border-radius: 4px; font-family: 'Courier New',Courier; font-size: 9px; color: #F0F0F0}
.MathJax_Menu_Button .MathJax_Hover_Arrow span {display: block; background-color: #AAA; border: 1px solid; border-radius: 3px; line-height: 0; padding: 4px}
.MathJax_Hover_Arrow:hover {color: white!important; border: 2px solid #CCC!important}
.MathJax_Hover_Arrow:hover span {background-color: #CCC!important}
</style><style type="text/css">#MathJax_About {position: fixed; left: 50%; width: auto; text-align: center; border: 3px outset; padding: 1em 2em; background-color: #DDDDDD; color: black; cursor: default; font-family: message-box; font-size: 120%; font-style: normal; text-indent: 0; text-transform: none; line-height: normal; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; z-index: 201; border-radius: 15px; -webkit-border-radius: 15px; -moz-border-radius: 15px; -khtml-border-radius: 15px; box-shadow: 0px 10px 20px #808080; -webkit-box-shadow: 0px 10px 20px #808080; -moz-box-shadow: 0px 10px 20px #808080; -khtml-box-shadow: 0px 10px 20px #808080; filter: progid:DXImageTransform.Microsoft.dropshadow(OffX=2, OffY=2, Color='gray', Positive='true')}
#MathJax_About.MathJax_MousePost {outline: none}
.MathJax_Menu {position: absolute; background-color: white; color: black; width: auto; padding: 2px; border: 1px solid #CCCCCC; margin: 0; cursor: default; font: menu; text-align: left; text-indent: 0; text-transform: none; line-height: normal; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; z-index: 201; box-shadow: 0px 10px 20px #808080; -webkit-box-shadow: 0px 10px 20px #808080; -moz-box-shadow: 0px 10px 20px #808080; -khtml-box-shadow: 0px 10px 20px #808080; filter: progid:DXImageTransform.Microsoft.dropshadow(OffX=2, OffY=2, Color='gray', Positive='true')}
.MathJax_MenuItem {padding: 2px 2em; background: transparent}
.MathJax_MenuArrow {position: absolute; right: .5em; padding-top: .25em; color: #666666; font-size: .75em}
.MathJax_MenuActive .MathJax_MenuArrow {color: white}
.MathJax_MenuArrow.RTL {left: .5em; right: auto}
.MathJax_MenuCheck {position: absolute; left: .7em}
.MathJax_MenuCheck.RTL {right: .7em; left: auto}
.MathJax_MenuRadioCheck {position: absolute; left: 1em}
.MathJax_MenuRadioCheck.RTL {right: 1em; left: auto}
.MathJax_MenuLabel {padding: 2px 2em 4px 1.33em; font-style: italic}
.MathJax_MenuRule {border-top: 1px solid #CCCCCC; margin: 4px 1px 0px}
.MathJax_MenuDisabled {color: GrayText}
.MathJax_MenuActive {background-color: Highlight; color: HighlightText}
.MathJax_MenuDisabled:focus, .MathJax_MenuLabel:focus {background-color: #E8E8E8}
.MathJax_ContextMenu:focus {outline: none}
.MathJax_ContextMenu .MathJax_MenuItem:focus {outline: none}
#MathJax_AboutClose {top: .2em; right: .2em}
.MathJax_Menu .MathJax_MenuClose {top: -10px; left: -10px}
.MathJax_MenuClose {position: absolute; cursor: pointer; display: inline-block; border: 2px solid #AAA; border-radius: 18px; -webkit-border-radius: 18px; -moz-border-radius: 18px; -khtml-border-radius: 18px; font-family: 'Courier New',Courier; font-size: 24px; color: #F0F0F0}
.MathJax_MenuClose span {display: block; background-color: #AAA; border: 1.5px solid; border-radius: 18px; -webkit-border-radius: 18px; -moz-border-radius: 18px; -khtml-border-radius: 18px; line-height: 0; padding: 8px 0 6px}
.MathJax_MenuClose:hover {color: white!important; border: 2px solid #CCC!important}
.MathJax_MenuClose:hover span {background-color: #CCC!important}
.MathJax_MenuClose:hover:focus {outline: none}
</style><style type="text/css">.MathJax_Preview .MJXf-math {color: inherit!important}
</style><style type="text/css">.MJX_Assistive_MathML {position: absolute!important; top: 0; left: 0; clip: rect(1px, 1px, 1px, 1px); padding: 1px 0 0 0!important; border: 0!important; height: 1px!important; width: 1px!important; overflow: hidden!important; display: block!important; -webkit-touch-callout: none; -webkit-user-select: none; -khtml-user-select: none; -moz-user-select: none; -ms-user-select: none; user-select: none}
.MJX_Assistive_MathML.MJX_Assistive_MathML_Block {width: 100%!important}
</style><style type="text/css">#MathJax_Zoom {position: absolute; background-color: #F0F0F0; overflow: auto; display: block; z-index: 301; padding: .5em; border: 1px solid black; margin: 0; font-weight: normal; font-style: normal; text-align: left; text-indent: 0; text-transform: none; line-height: normal; letter-spacing: normal; word-spacing: normal; word-wrap: normal; white-space: nowrap; float: none; -webkit-box-sizing: content-box; -moz-box-sizing: content-box; box-sizing: content-box; box-shadow: 5px 5px 15px #AAAAAA; -webkit-box-shadow: 5px 5px 15px #AAAAAA; -moz-box-shadow: 5px 5px 15px #AAAAAA; -khtml-box-shadow: 5px 5px 15px #AAAAAA; filter: progid:DXImageTransform.Microsoft.dropshadow(OffX=2, OffY=2, Color='gray', Positive='true')}
#MathJax_ZoomOverlay {position: absolute; left: 0; top: 0; z-index: 300; display: inline-block; width: 100%; height: 100%; border: 0; padding: 0; margin: 0; background-color: white; opacity: 0; filter: alpha(opacity=0)}
#MathJax_ZoomFrame {position: relative; display: inline-block; height: 0; width: 0}
#MathJax_ZoomEventTrap {position: absolute; left: 0; top: 0; z-index: 302; display: inline-block; border: 0; padding: 0; margin: 0; background-color: white; opacity: 0; filter: alpha(opacity=0)}
</style><style type="text/css">.MathJax_Preview {color: #888}
#MathJax_Message {position: fixed; left: 1em; bottom: 1.5em; background-color: #E6E6E6; border: 1px solid #959595; margin: 0px; padding: 2px 8px; z-index: 102; color: black; font-size: 80%; width: auto; white-space: nowrap}
#MathJax_MSIE_Frame {position: absolute; top: 0; left: 0; width: 0px; z-index: 101; border: 0px; margin: 0px; padding: 0px}
.MathJax_Error {color: #CC0000; font-style: italic}
</style><style type="text/css">.MJXp-script {font-size: .8em}
.MJXp-right {-webkit-transform-origin: right; -moz-transform-origin: right; -ms-transform-origin: right; -o-transform-origin: right; transform-origin: right}
.MJXp-bold {font-weight: bold}
.MJXp-italic {font-style: italic}
.MJXp-scr {font-family: MathJax_Script,'Times New Roman',Times,STIXGeneral,serif}
.MJXp-frak {font-family: MathJax_Fraktur,'Times New Roman',Times,STIXGeneral,serif}
.MJXp-sf {font-family: MathJax_SansSerif,'Times New Roman',Times,STIXGeneral,serif}
.MJXp-cal {font-family: MathJax_Caligraphic,'Times New Roman',Times,STIXGeneral,serif}
.MJXp-mono {font-family: MathJax_Typewriter,'Times New Roman',Times,STIXGeneral,serif}
.MJXp-largeop {font-size: 150%}
.MJXp-largeop.MJXp-int {vertical-align: -.2em}
.MJXp-math {display: inline-block; line-height: 1.2; text-indent: 0; font-family: 'Times New Roman',Times,STIXGeneral,serif; white-space: nowrap; border-collapse: collapse}
.MJXp-display {display: block; text-align: center; margin: 1em 0}
.MJXp-math span {display: inline-block}
.MJXp-box {display: block!important; text-align: center}
.MJXp-box:after {content: " "}
.MJXp-rule {display: block!important; margin-top: .1em}
.MJXp-char {display: block!important}
.MJXp-mo {margin: 0 .15em}
.MJXp-mfrac {margin: 0 .125em; vertical-align: .25em}
.MJXp-denom {display: inline-table!important; width: 100%}
.MJXp-denom > * {display: table-row!important}
.MJXp-surd {vertical-align: top}
.MJXp-surd > * {display: block!important}
.MJXp-script-box > *  {display: table!important; height: 50%}
.MJXp-script-box > * > * {display: table-cell!important; vertical-align: top}
.MJXp-script-box > *:last-child > * {vertical-align: bottom}
.MJXp-script-box > * > * > * {display: block!important}
.MJXp-mphantom {visibility: hidden}
.MJXp-munderover {display: inline-table!important}
.MJXp-over {display: inline-block!important; text-align: center}
.MJXp-over > * {display: block!important}
.MJXp-munderover > * {display: table-row!important}
.MJXp-mtable {vertical-align: .25em; margin: 0 .125em}
.MJXp-mtable > * {display: inline-table!important; vertical-align: middle}
.MJXp-mtr {display: table-row!important}
.MJXp-mtd {display: table-cell!important; text-align: center; padding: .5em 0 0 .5em}
.MJXp-mtr > .MJXp-mtd:first-child {padding-left: 0}
.MJXp-mtr:first-child > .MJXp-mtd {padding-top: 0}
.MJXp-mlabeledtr {display: table-row!important}
.MJXp-mlabeledtr > .MJXp-mtd:first-child {padding-left: 0}
.MJXp-mlabeledtr:first-child > .MJXp-mtd {padding-top: 0}
.MJXp-merror {background-color: #FFFF88; color: #CC0000; border: 1px solid #CC0000; padding: 1px 3px; font-style: normal; font-size: 90%}
.MJXp-scale0 {-webkit-transform: scaleX(.0); -moz-transform: scaleX(.0); -ms-transform: scaleX(.0); -o-transform: scaleX(.0); transform: scaleX(.0)}
.MJXp-scale1 {-webkit-transform: scaleX(.1); -moz-transform: scaleX(.1); -ms-transform: scaleX(.1); -o-transform: scaleX(.1); transform: scaleX(.1)}
.MJXp-scale2 {-webkit-transform: scaleX(.2); -moz-transform: scaleX(.2); -ms-transform: scaleX(.2); -o-transform: scaleX(.2); transform: scaleX(.2)}
.MJXp-scale3 {-webkit-transform: scaleX(.3); -moz-transform: scaleX(.3); -ms-transform: scaleX(.3); -o-transform: scaleX(.3); transform: scaleX(.3)}
.MJXp-scale4 {-webkit-transform: scaleX(.4); -moz-transform: scaleX(.4); -ms-transform: scaleX(.4); -o-transform: scaleX(.4); transform: scaleX(.4)}
.MJXp-scale5 {-webkit-transform: scaleX(.5); -moz-transform: scaleX(.5); -ms-transform: scaleX(.5); -o-transform: scaleX(.5); transform: scaleX(.5)}
.MJXp-scale6 {-webkit-transform: scaleX(.6); -moz-transform: scaleX(.6); -ms-transform: scaleX(.6); -o-transform: scaleX(.6); transform: scaleX(.6)}
.MJXp-scale7 {-webkit-transform: scaleX(.7); -moz-transform: scaleX(.7); -ms-transform: scaleX(.7); -o-transform: scaleX(.7); transform: scaleX(.7)}
.MJXp-scale8 {-webkit-transform: scaleX(.8); -moz-transform: scaleX(.8); -ms-transform: scaleX(.8); -o-transform: scaleX(.8); transform: scaleX(.8)}
.MJXp-scale9 {-webkit-transform: scaleX(.9); -moz-transform: scaleX(.9); -ms-transform: scaleX(.9); -o-transform: scaleX(.9); transform: scaleX(.9)}
.MathJax_PHTML .noError {vertical-align: ; font-size: 90%; text-align: left; color: black; padding: 1px 3px; border: 1px solid}
</style><style id="plotly.js-style-global"></style><style id="plotly.js-style-modebar-69ac2d"></style><style id="plotly.js-style-modebar-fa0e2b"></style><style id="plotly.js-style-modebar-971995"></style><style id="plotly.js-style-modebar-02f7ae"></style><style id="plotly.js-style-modebar-d9cdc6"></style><style id="plotly.js-style-modebar-4cfd16"></style></head>
<body style=""><div id="MathJax_Message" style="display: none;"></div>
  <div tabindex="-1" id="notebook" class="border-box-sizing">
    <div class="container" id="notebook-container">

<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="소개">소개<a class="anchor-link" href="#소개">¶</a></h2><p>이 노트북은 학습 모델을 앙상블(결합)하는 방법, 특히 스태킹(stacking)이라고 알려진 앙상블 기법의 변형에 대해 매우 기본적이고 간단한 소개를 하고 있습니다. 간략하게 말하자면, 첫 번째 레벨(base)에서 사용되는 스태킹은 몇 가지 기본 분류기를 통해 예측을 수행한 다음 두 번째 레벨에서 다른 모델을 사용해 첫 번째 레벨에서 나온 예측 결과를 다시 예측합니다.</p>
<p>타이타닉 데이터세트는 Kaggle의 많은 신규 사용자들이 접하는 데이터셋임과 동시에 스태킹 개념을 도입한 첫 번째 후보 데이터셋입니다. 스태킹 기법이 Kaggle 대회들에 있어서 많은 우승 팀의 주요 솔루션으로 쓰였음에도 이 주제에 대한 커널이 많이 부족하기 때문에, 이 노트북이 그 공백을 어느 정도 채울 수 있기를 바랍니다.</p>
<p>저 또한 Kaggle 초보자입니다. 그럼에도 제가 기회를 만들고 공부할 수 있게 해 준 앙상블/스태킹 스크립트는 AllState Severity Claims 대회에서 우수한 성적을 거둔 Faron이 만들었습니다. 이 노트북의 내용은 Faron의 스크립트로부터 타이타닉 문제가 분류기들에 대한 앙상블임을 고려하여 이식되었지만 Faron은 회귀를 위한 앙상블을 작업했습니다. 여기에서 Faron의 스크립트를 확인할 수 있습니다 :</p>
<p><a href="https://www.kaggle.com/mmueller/allstate-claims-severity/stacking-starter/run/390867">Stacking Starter</a> : by Faron</p>
<p>여러분들이 이 노트북을 통해 앙상블의 정의와 개념을 잘 전달받기를 바랍니다. 아래에서 언급할 앙상블 기법을 구현한 다른 독립형 kaggle 스크립트는 Public LB 점수가 0.808로 상위 9%였으며 4분 이내에 실행됩니다. 그럼에도 불구하고 저는 이 스크립트를 개선하고 추가할 여지가 많다는 것을 확신합니다. 제가 어떻게 더 향상했으면 좋겠는지에 대한 의견을 남겨 주십시오.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[1]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Load in our libraries</span>
<span class="kn">import</span> <span class="nn">pandas</span> <span class="k">as</span> <span class="nn">pd</span>
<span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>
<span class="kn">import</span> <span class="nn">re</span>
<span class="kn">import</span> <span class="nn">sklearn</span>
<span class="kn">import</span> <span class="nn">xgboost</span> <span class="k">as</span> <span class="nn">xgb</span>
<span class="kn">import</span> <span class="nn">seaborn</span> <span class="k">as</span> <span class="nn">sns</span>
<span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="k">as</span> <span class="nn">plt</span>
<span class="o">%</span><span class="k">matplotlib</span> inline

<span class="kn">import</span> <span class="nn">plotly.offline</span> <span class="k">as</span> <span class="nn">py</span>
<span class="n">py</span><span class="o">.</span><span class="n">init_notebook_mode</span><span class="p">(</span><span class="n">connected</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
<span class="kn">import</span> <span class="nn">plotly.graph_objs</span> <span class="k">as</span> <span class="nn">go</span>
<span class="kn">import</span> <span class="nn">plotly.tools</span> <span class="k">as</span> <span class="nn">tls</span>

<span class="kn">import</span> <span class="nn">warnings</span>
<span class="n">warnings</span><span class="o">.</span><span class="n">filterwarnings</span><span class="p">(</span><span class="s1">'ignore'</span><span class="p">)</span>

<span class="c1"># Going to use these 5 base models for the stacking</span>
<span class="kn">from</span> <span class="nn">sklearn.ensemble</span> <span class="k">import</span> <span class="p">(</span><span class="n">RandomForestClassifier</span><span class="p">,</span> <span class="n">AdaBoostClassifier</span><span class="p">,</span> 
                              <span class="n">GradientBoostingClassifier</span><span class="p">,</span> <span class="n">ExtraTreesClassifier</span><span class="p">)</span>

<span class="kn">from</span> <span class="nn">sklearn.model_selection</span> <span class="k">import</span> <span class="n">GridSearchCV</span><span class="p">,</span> <span class="n">cross_val_score</span><span class="p">,</span> <span class="n">learning_curve</span>
<span class="c1"># from sklearn.model_selection import StratifiedKFold</span>
<span class="kn">from</span> <span class="nn">sklearn.model_selection</span> <span class="k">import</span> <span class="n">KFold</span>
<span class="kn">from</span> <span class="nn">sklearn.svm</span> <span class="k">import</span> <span class="n">SVC</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>



<div class="output_html rendered_html output_subarea ">
        <script type="text/javascript">
        window.PlotlyConfig = {MathJaxConfig: 'local'};
        if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
        if (typeof require !== 'undefined') {
        require.undef("plotly");
        requirejs.config({
            paths: {
                'plotly': ['https://cdn.plot.ly/plotly-latest.min']
            }
        });
        require(['plotly'], function(Plotly) {
            window._Plotly = Plotly;
        });
        }
        </script>
        
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Feature-Exploration,-Engineering-and-Cleaning">Feature Exploration, Engineering and Cleaning<a class="anchor-link" href="#Feature-Exploration,-Engineering-and-Cleaning">¶</a></h2><p>이제부터 우리는 대부분의 커널들이 진행하는 것처럼 진행할 것입니다. 먼저 손에 들고 있는 데이터를 탐색하고 가능한 feature engineering 방법들에 대해 확인한 후 수치형 데이터로 모든 범주형 특징(categorical features)들을 인코딩할 것입니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[2]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Load in the train and test datasets</span>
<span class="n">train</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="s1">'./train.csv'</span><span class="p">)</span>
<span class="n">test</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">read_csv</span><span class="p">(</span><span class="s1">'./test.csv'</span><span class="p">)</span>

<span class="c1"># Store our passenger ID for easy access</span>
<span class="n">PassengerId</span> <span class="o">=</span> <span class="n">test</span><span class="p">[</span><span class="s1">'PassengerId'</span><span class="p">]</span>

<span class="n">train</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">3</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt output_prompt">Out[2]:</div>



<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
      <th>Cabin</th>
      <th>Embarked</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
      <td>C85</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
      <td>NaN</td>
      <td>S</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>범주형 자료에서 어떤 식으로든 정보를 추출하는 것이 놀라운 일은 아닙니다. 
Sina는 feature engineering에 대한 매우 포괄적이고 잘 구성된 노트북을 갖고 있습니다. 그의 작업물을 확인해 보십시오.</p>
<p><a href="https://www.kaggle.com/sinakhorami/titanic/titanic-best-working-classifier">Titanic Best Working Classfier</a> : by Sina</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[3]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">full_data</span> <span class="o">=</span> <span class="p">[</span><span class="n">train</span><span class="p">,</span> <span class="n">test</span><span class="p">]</span>

<span class="c1"># Some features of my own that I have added in Gives the length of the name</span>
<span class="n">train</span><span class="p">[</span><span class="s1">'Name_length'</span><span class="p">]</span> <span class="o">=</span> <span class="n">train</span><span class="p">[</span><span class="s1">'Name'</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="nb">len</span><span class="p">)</span>
<span class="n">test</span><span class="p">[</span><span class="s1">'Name_length'</span><span class="p">]</span> <span class="o">=</span> <span class="n">test</span><span class="p">[</span><span class="s1">'Name'</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="nb">len</span><span class="p">)</span>

<span class="c1"># 승객이 타이타닉 호에서 객실을 배정받았는지 알려주는 lambda함수를 통해 Has_Cabin이라는 feature를 만듭니다. </span>
<span class="n">train</span><span class="p">[</span><span class="s1">'Has_Cabin'</span><span class="p">]</span> <span class="o">=</span> <span class="n">train</span><span class="p">[</span><span class="s2">"Cabin"</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="mi">0</span> <span class="k">if</span> <span class="nb">type</span><span class="p">(</span><span class="n">x</span><span class="p">)</span> <span class="o">==</span> <span class="nb">float</span> <span class="k">else</span> <span class="mi">1</span><span class="p">)</span>
<span class="n">test</span><span class="p">[</span><span class="s1">'Has_Cabin'</span><span class="p">]</span> <span class="o">=</span> <span class="n">test</span><span class="p">[</span><span class="s2">"Cabin"</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="k">lambda</span> <span class="n">x</span><span class="p">:</span> <span class="mi">0</span> <span class="k">if</span> <span class="nb">type</span><span class="p">(</span><span class="n">x</span><span class="p">)</span> <span class="o">==</span> <span class="nb">float</span> <span class="k">else</span> <span class="mi">1</span><span class="p">)</span>

<span class="c1"># 아래는 Sina의 노트북으로부터 가져온 Feature Engineering 단계들입니다.</span>

<span class="c1"># SibSp와 Parch를 합쳐 FamilySize라는 새 feature를 만듭니다. </span>
<span class="k">for</span> <span class="n">dataset</span> <span class="ow">in</span> <span class="n">full_data</span><span class="p">:</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'FamilySize'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'SibSp'</span><span class="p">]</span> <span class="o">+</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Parch'</span><span class="p">]</span> <span class="o">+</span> <span class="mi">1</span>
    
<span class="c1"># FamilySize로부터 IsAlone이라는 새로운 feature를 만듭니다. </span>
<span class="k">for</span> <span class="n">dataset</span> <span class="ow">in</span> <span class="n">full_data</span><span class="p">:</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'IsAlone'</span><span class="p">]</span> <span class="o">=</span> <span class="mi">0</span>
    <span class="n">dataset</span><span class="o">.</span><span class="n">loc</span><span class="p">[</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'FamilySize'</span><span class="p">]</span> <span class="o">==</span> <span class="mi">1</span><span class="p">,</span> <span class="s1">'IsAlone'</span><span class="p">]</span> <span class="o">=</span> <span class="mi">1</span>

<span class="c1"># Embarked 열에 존재하는 모든 NULL값들을 제거합니다. </span>
<span class="k">for</span> <span class="n">dataset</span> <span class="ow">in</span> <span class="n">full_data</span><span class="p">:</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Embarked'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Embarked'</span><span class="p">]</span><span class="o">.</span><span class="n">fillna</span><span class="p">(</span><span class="s1">'S'</span><span class="p">)</span>
    
<span class="c1"># Fare 열에 존재하는 NULL값을 모두 제거하고 CategoricalFare라는 새로운 feature를 만듭니다. </span>
<span class="k">for</span> <span class="n">dataset</span> <span class="ow">in</span> <span class="n">full_data</span><span class="p">:</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">]</span><span class="o">.</span><span class="n">fillna</span><span class="p">(</span><span class="n">train</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">]</span><span class="o">.</span><span class="n">median</span><span class="p">())</span>
<span class="n">train</span><span class="p">[</span><span class="s1">'CategoricalFare'</span><span class="p">]</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">qcut</span><span class="p">(</span><span class="n">train</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">],</span> <span class="mi">4</span><span class="p">)</span>

<span class="c1"># CategoricalAge이라는 새로운 feature를 만듭니다. </span>
<span class="k">for</span> <span class="n">dataset</span> <span class="ow">in</span> <span class="n">full_data</span><span class="p">:</span>
    <span class="n">age_avg</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span><span class="o">.</span><span class="n">mean</span><span class="p">()</span>
    <span class="n">age_std</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span><span class="o">.</span><span class="n">std</span><span class="p">()</span>
    <span class="n">age_null_count</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span><span class="o">.</span><span class="n">isnull</span><span class="p">()</span><span class="o">.</span><span class="n">sum</span><span class="p">()</span>
    <span class="n">age_null_random_list</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randint</span><span class="p">(</span><span class="n">age_avg</span> <span class="o">-</span> <span class="n">age_std</span><span class="p">,</span> <span class="n">age_avg</span> <span class="o">+</span> <span class="n">age_std</span><span class="p">,</span> <span class="n">size</span><span class="o">=</span><span class="n">age_null_count</span><span class="p">)</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">][</span><span class="n">np</span><span class="o">.</span><span class="n">isnan</span><span class="p">(</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">])]</span> <span class="o">=</span> <span class="n">age_null_random_list</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="nb">int</span><span class="p">)</span>
<span class="n">train</span><span class="p">[</span><span class="s1">'CategoricalAge'</span><span class="p">]</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">cut</span><span class="p">(</span><span class="n">train</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">],</span> <span class="mi">5</span><span class="p">)</span>

<span class="c1"># 승객 이름에서 칭호를 추출하는 함수를 정의합니다. </span>
<span class="k">def</span> <span class="nf">get_title</span><span class="p">(</span><span class="n">name</span><span class="p">):</span>
    <span class="n">title_search</span> <span class="o">=</span> <span class="n">re</span><span class="o">.</span><span class="n">search</span><span class="p">(</span><span class="s1">' ([A-Za-z]+)\.'</span><span class="p">,</span> <span class="n">name</span><span class="p">)</span>
    <span class="c1"># If the title exists, extract and return it.</span>
    <span class="k">if</span> <span class="n">title_search</span><span class="p">:</span>
        <span class="k">return</span> <span class="n">title_search</span><span class="o">.</span><span class="n">group</span><span class="p">(</span><span class="mi">1</span><span class="p">)</span>
    <span class="k">return</span> <span class="s2">""</span>

<span class="c1"># 승객들의 호칭을 담고 있는 새로운 피쳐, Title을 만듭니다. </span>
<span class="k">for</span> <span class="n">dataset</span> <span class="ow">in</span> <span class="n">full_data</span><span class="p">:</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Name'</span><span class="p">]</span><span class="o">.</span><span class="n">apply</span><span class="p">(</span><span class="n">get_title</span><span class="p">)</span>
    
<span class="c1"># 잘 알려지지 않은 나머지 호칭들은 "Rare"이라는 그룹으로 묶습니다. </span>
<span class="k">for</span> <span class="n">dataset</span> <span class="ow">in</span> <span class="n">full_data</span><span class="p">:</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span><span class="o">.</span><span class="n">replace</span><span class="p">([</span><span class="s1">'Lady'</span><span class="p">,</span> <span class="s1">'Countess'</span><span class="p">,</span><span class="s1">'Capt'</span><span class="p">,</span> <span class="s1">'Col'</span><span class="p">,</span><span class="s1">'Don'</span><span class="p">,</span> <span class="s1">'Dr'</span><span class="p">,</span> <span class="s1">'Major'</span><span class="p">,</span> 
                                                 <span class="s1">'Rev'</span><span class="p">,</span> <span class="s1">'Sir'</span><span class="p">,</span> <span class="s1">'Jonkheer'</span><span class="p">,</span> <span class="s1">'Dona'</span><span class="p">],</span> <span class="s1">'Rare'</span><span class="p">)</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span><span class="o">.</span><span class="n">replace</span><span class="p">(</span><span class="s1">'Mlle'</span><span class="p">,</span> <span class="s1">'Miss'</span><span class="p">)</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span><span class="o">.</span><span class="n">replace</span><span class="p">(</span><span class="s1">'Ms'</span><span class="p">,</span> <span class="s1">'Miss'</span><span class="p">)</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span><span class="o">.</span><span class="n">replace</span><span class="p">(</span><span class="s1">'Mme'</span><span class="p">,</span> <span class="s1">'Mrs'</span><span class="p">)</span>

<span class="k">for</span> <span class="n">dataset</span> <span class="ow">in</span> <span class="n">full_data</span><span class="p">:</span>
    <span class="c1"># Mapping Sex</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Sex'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Sex'</span><span class="p">]</span><span class="o">.</span><span class="n">map</span><span class="p">(</span> <span class="p">{</span><span class="s1">'female'</span><span class="p">:</span> <span class="mi">0</span><span class="p">,</span> <span class="s1">'male'</span><span class="p">:</span> <span class="mi">1</span><span class="p">}</span> <span class="p">)</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="nb">int</span><span class="p">)</span>
    
    <span class="c1"># Mapping titles</span>
    <span class="n">title_mapping</span> <span class="o">=</span> <span class="p">{</span><span class="s2">"Mr"</span><span class="p">:</span> <span class="mi">1</span><span class="p">,</span> <span class="s2">"Miss"</span><span class="p">:</span> <span class="mi">2</span><span class="p">,</span> <span class="s2">"Mrs"</span><span class="p">:</span> <span class="mi">3</span><span class="p">,</span> <span class="s2">"Master"</span><span class="p">:</span> <span class="mi">4</span><span class="p">,</span> <span class="s2">"Rare"</span><span class="p">:</span> <span class="mi">5</span><span class="p">}</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span><span class="o">.</span><span class="n">map</span><span class="p">(</span><span class="n">title_mapping</span><span class="p">)</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Title'</span><span class="p">]</span><span class="o">.</span><span class="n">fillna</span><span class="p">(</span><span class="mi">0</span><span class="p">)</span>
    
    <span class="c1"># Mapping Embarked</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Embarked'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Embarked'</span><span class="p">]</span><span class="o">.</span><span class="n">map</span><span class="p">(</span> <span class="p">{</span><span class="s1">'S'</span><span class="p">:</span> <span class="mi">0</span><span class="p">,</span> <span class="s1">'C'</span><span class="p">:</span> <span class="mi">1</span><span class="p">,</span> <span class="s1">'Q'</span><span class="p">:</span> <span class="mi">2</span><span class="p">}</span> <span class="p">)</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="nb">int</span><span class="p">)</span>
    
    <span class="c1"># Mapping Fare</span>
    <span class="n">dataset</span><span class="o">.</span><span class="n">loc</span><span class="p">[</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">]</span> <span class="o">&lt;=</span> <span class="mf">7.91</span><span class="p">,</span> <span class="s1">'Fare'</span><span class="p">]</span> <span class="o">=</span> <span class="mi">0</span>
    <span class="n">dataset</span><span class="o">.</span><span class="n">loc</span><span class="p">[(</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">]</span> <span class="o">&gt;</span> <span class="mf">7.91</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">]</span> <span class="o">&lt;=</span> <span class="mf">14.454</span><span class="p">),</span> <span class="s1">'Fare'</span><span class="p">]</span> <span class="o">=</span> <span class="mi">1</span>
    <span class="n">dataset</span><span class="o">.</span><span class="n">loc</span><span class="p">[(</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">]</span> <span class="o">&gt;</span> <span class="mf">14.454</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">]</span> <span class="o">&lt;=</span> <span class="mi">31</span><span class="p">),</span> <span class="s1">'Fare'</span><span class="p">]</span>   <span class="o">=</span> <span class="mi">2</span>
    <span class="n">dataset</span><span class="o">.</span><span class="n">loc</span><span class="p">[</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">]</span> <span class="o">&gt;</span> <span class="mi">31</span><span class="p">,</span> <span class="s1">'Fare'</span><span class="p">]</span> <span class="o">=</span> <span class="mi">3</span>
    <span class="n">dataset</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">]</span> <span class="o">=</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Fare'</span><span class="p">]</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="nb">int</span><span class="p">)</span>
    
    <span class="c1"># Mapping Age</span>
    <span class="n">dataset</span><span class="o">.</span><span class="n">loc</span><span class="p">[</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span> <span class="o">&lt;=</span> <span class="mi">16</span><span class="p">,</span> <span class="s1">'Age'</span><span class="p">]</span> <span class="o">=</span> <span class="mi">0</span>
    <span class="n">dataset</span><span class="o">.</span><span class="n">loc</span><span class="p">[(</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span> <span class="o">&gt;</span> <span class="mi">16</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span> <span class="o">&lt;=</span> <span class="mi">32</span><span class="p">),</span> <span class="s1">'Age'</span><span class="p">]</span> <span class="o">=</span> <span class="mi">1</span>
    <span class="n">dataset</span><span class="o">.</span><span class="n">loc</span><span class="p">[(</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span> <span class="o">&gt;</span> <span class="mi">32</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span> <span class="o">&lt;=</span> <span class="mi">48</span><span class="p">),</span> <span class="s1">'Age'</span><span class="p">]</span> <span class="o">=</span> <span class="mi">2</span>
    <span class="n">dataset</span><span class="o">.</span><span class="n">loc</span><span class="p">[(</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span> <span class="o">&gt;</span> <span class="mi">48</span><span class="p">)</span> <span class="o">&amp;</span> <span class="p">(</span><span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span> <span class="o">&lt;=</span> <span class="mi">64</span><span class="p">),</span> <span class="s1">'Age'</span><span class="p">]</span> <span class="o">=</span> <span class="mi">3</span>
    <span class="n">dataset</span><span class="o">.</span><span class="n">loc</span><span class="p">[</span> <span class="n">dataset</span><span class="p">[</span><span class="s1">'Age'</span><span class="p">]</span> <span class="o">&gt;</span> <span class="mi">64</span><span class="p">,</span> <span class="s1">'Age'</span><span class="p">]</span> <span class="o">=</span> <span class="mi">4</span> <span class="p">;</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[4]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Feature selection</span>
<span class="n">drop_elements</span> <span class="o">=</span> <span class="p">[</span><span class="s1">'PassengerId'</span><span class="p">,</span> <span class="s1">'Name'</span><span class="p">,</span> <span class="s1">'Ticket'</span><span class="p">,</span> <span class="s1">'Cabin'</span><span class="p">,</span> <span class="s1">'SibSp'</span><span class="p">]</span>
<span class="n">train</span> <span class="o">=</span> <span class="n">train</span><span class="o">.</span><span class="n">drop</span><span class="p">(</span><span class="n">drop_elements</span><span class="p">,</span> <span class="n">axis</span> <span class="o">=</span> <span class="mi">1</span><span class="p">)</span>
<span class="n">train</span> <span class="o">=</span> <span class="n">train</span><span class="o">.</span><span class="n">drop</span><span class="p">([</span><span class="s1">'CategoricalAge'</span><span class="p">,</span> <span class="s1">'CategoricalFare'</span><span class="p">],</span> <span class="n">axis</span> <span class="o">=</span> <span class="mi">1</span><span class="p">)</span>
<span class="n">test</span>  <span class="o">=</span> <span class="n">test</span><span class="o">.</span><span class="n">drop</span><span class="p">(</span><span class="n">drop_elements</span><span class="p">,</span> <span class="n">axis</span> <span class="o">=</span> <span class="mi">1</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>좋습니다. features를 정리하고 연관 있는(relevant) 정보들을 추출한 후 카테고리 열(categorical columns)들을 삭제했으므로 우리가 갖고 있는 features는 모두 숫자형 자료여야 합니다. 숫자형 자료는 우리의 기계 학습 모델들에 적합한 형식입니다. 그러나 진행하기에 앞서 변경된 데이터셋으로 간단한 상관 관계 및 분포표를 그려 보도록 하겠습니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="시각화">시각화<a class="anchor-link" href="#시각화">¶</a></h2>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[5]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">train</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">3</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt output_prompt">Out[5]:</div>



<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Parch</th>
      <th>Fare</th>
      <th>Embarked</th>
      <th>Name_length</th>
      <th>Has_Cabin</th>
      <th>FamilySize</th>
      <th>IsAlone</th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>23</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>51</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>22</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="피어슨-상관관계-히트맵(Pearson-Correlation-Heatmap)">피어슨 상관관계 히트맵(Pearson Correlation Heatmap)<a class="anchor-link" href="#피어슨-상관관계-히트맵(Pearson-Correlation-Heatmap)">¶</a></h3><p>우리는 이제부터 몇 가지 상관 관계 플롯을 생성하여 서로 연관된 feaures끼리 어떻게 나타나는지 살펴볼 것입니다. 그러기 위해 Seaborn 패키지를 활용해 히트맵을 다음과 같이 매우 편리하게 그려 볼 수 있습니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[6]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">colormap</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">cm</span><span class="o">.</span><span class="n">RdBu</span>
<span class="n">plt</span><span class="o">.</span><span class="n">figure</span><span class="p">(</span><span class="n">figsize</span><span class="o">=</span><span class="p">(</span><span class="mi">14</span><span class="p">,</span><span class="mi">12</span><span class="p">))</span>
<span class="n">plt</span><span class="o">.</span><span class="n">title</span><span class="p">(</span><span class="s1">'Pearson Correlation of Features'</span><span class="p">,</span> <span class="n">y</span><span class="o">=</span><span class="mf">1.05</span><span class="p">,</span> <span class="n">size</span><span class="o">=</span><span class="mi">15</span><span class="p">)</span>
<span class="n">sns</span><span class="o">.</span><span class="n">heatmap</span><span class="p">(</span><span class="n">train</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="nb">float</span><span class="p">)</span><span class="o">.</span><span class="n">corr</span><span class="p">(),</span><span class="n">linewidths</span><span class="o">=</span><span class="mf">0.1</span><span class="p">,</span><span class="n">vmax</span><span class="o">=</span><span class="mf">1.0</span><span class="p">,</span> 
            <span class="n">square</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span> <span class="n">cmap</span><span class="o">=</span><span class="n">colormap</span><span class="p">,</span> <span class="n">linecolor</span><span class="o">=</span><span class="s1">'white'</span><span class="p">,</span> <span class="n">annot</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt output_prompt">Out[6]:</div>




<div class="output_text output_subarea output_execute_result">
<pre>&lt;matplotlib.axes._subplots.AxesSubplot at 0x22cd88b0c18&gt;</pre>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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CfKZzc2DUe59zDXGTMM5zpFAV+BDa+2Pxpg5OI2+9e4Yrsf57v/pbx4LEZFiQY0XESkS1trlxpgOOFPnfoxzx/9PnMZGRn/+53CmmH3e/Xw28Aju/+NyFptwMgYv41wEHsKZGvnZbGXuBcbjDMoujzPIv5c9t39meFbW2i/dd86H4UyvXBE4gjMl8BXZij6LMyvXQJxuT7uBW621n57Ha1tjzHU400A/inORHY8zYPv1v9i0P8400NNxurG9gTOhwEPZyizDGRMzCGdK45+ATh5iOGOMuQLnPXgfZ4KEH4HrztKV7pxYaxcYY24GRuA0EmJxxpuMPN995/N6BXLOWmuTjTH3uuNcijNmxlhrU4wxfXDG1XyBM9HFQJwB/+cS32/GmMvcrz0VJwt0ECcjs9td7GecxvNQnN4W24C+f5GNExEp1oy15zI5joiIiIiIiHdpzIuIiIiIiPgENV5ERERERMQnqPEiIiIiIiI+QY0XERERERHxCWq8iIiIiIiIT1DjRUREREREfIIaLyIiIiIi4hPUeBEREREREZ+gxouIiIiIiPgENV5ERERERMQnqPEiIiIiIiI+QY0XERERERHxCWq8iIiIiIiIT1DjRUREREREfIIaLyIiIiIi4hPUeBEREREREZ+gxouIiIiIiPgENV5ERERERMQnqPEiIiIiIiI+QY0XERERERHxCWq8iIiIiIiIT1DjRUREREREfEKAtwMQEREREZGcgprdZb35+inrpxlvvn5+lHkRERERERGfoMaLiIiIiIj4BHUbExEREREpZoyfv7dDKJaUeREREREREZ+gzIuIiIiISDGjzItnyryIiIiIiIhPUONFRERERER8grqNiYiIiIgUM+o25pkyLyIiIiIi4hOUeRERERERKWaUefFMmRcREREREfEJaryIiIiIiIhPULexgmG9HYCIiIiI/G3G2wHkx/ir25gnarwUgKBmd3k7hAKTsn4aewb393YYBabW5BnMrd7E22EUmKv2b8K1Y5m3wygw/vXbk3LssLfDKDBBFSIA2HoowcuRFJxGVUI4k3jc22EUmBLB5Vm975i3wygwLS+owIaD/573p2lkedIObvd2GAUmILIBp2Y+7+0wCkyZfiP4cnO0t8MoMH0vqurtEOQfULcxERERERHxCcq8iIiIiIgUM36abcwjZV5ERERERMQnKPMi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
">
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="플롯으로부터-얻을-수-있는-점">플롯으로부터 얻을 수 있는 점<a class="anchor-link" href="#플롯으로부터-얻을-수-있는-점">¶</a></h3><p>피어슨 상관관계 플롯에서 우리는 서로 강하게 연관된 features가 많지 않다는 것을 알 수 있습니다. 이는 모델을 학습할 때 중복되거나 불필요한 데이터가 없기 때문에 모델에 features를 넣어 주기 좋습니다. 무엇보다도 각 feature에 고유한 정보들이 포함되어 있어 다행입니다. Family Size(가족 크기) 및 Parch(부모 및 자녀 수) 두 feature는 서로 강하게 연관되어 있습니다만 연습을 위해 이 두 feature는 모두 남겨 놓도록 하겠습니다.</p>
<h3 id="쌍을-이루는-플롯-그리기(Pairplots)">쌍을 이루는 플롯 그리기(Pairplots)<a class="anchor-link" href="#쌍을-이루는-플롯-그리기(Pairplots)">¶</a></h3><p>마지막으로 하나의 feature가 다른 feature에 대해 갖는 데이터 분포를 관찰하기 위해 몇 개의 쌍 플롯(Pairplots)을 그려 보도록 하겠습니다. 다시 한번 우리는 Seaborn 패키지를 사용할 것입니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[7]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">g</span> <span class="o">=</span> <span class="n">sns</span><span class="o">.</span><span class="n">pairplot</span><span class="p">(</span><span class="n">train</span><span class="p">[[</span><span class="sa">u</span><span class="s1">'Survived'</span><span class="p">,</span> <span class="sa">u</span><span class="s1">'Pclass'</span><span class="p">,</span> <span class="sa">u</span><span class="s1">'Sex'</span><span class="p">,</span> <span class="sa">u</span><span class="s1">'Age'</span><span class="p">,</span> <span class="sa">u</span><span class="s1">'Parch'</span><span class="p">,</span> <span class="sa">u</span><span class="s1">'Fare'</span><span class="p">,</span> <span class="sa">u</span><span class="s1">'Embarked'</span><span class="p">,</span>
       <span class="sa">u</span><span class="s1">'FamilySize'</span><span class="p">,</span> <span class="sa">u</span><span class="s1">'Title'</span><span class="p">]],</span> <span class="n">hue</span><span class="o">=</span><span class="s1">'Survived'</span><span class="p">,</span> <span class="n">palette</span> <span class="o">=</span> <span class="s1">'seismic'</span><span class="p">,</span><span class="n">size</span><span class="o">=</span><span class="mf">1.2</span><span class="p">,</span><span class="n">diag_kind</span> <span class="o">=</span> <span class="s1">'kde'</span><span class="p">,</span><span class="n">diag_kws</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span><span class="n">shade</span><span class="o">=</span><span class="kc">True</span><span class="p">),</span><span class="n">plot_kws</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span><span class="n">s</span><span class="o">=</span><span class="mi">10</span><span class="p">)</span> <span class="p">)</span>
<span class="n">g</span><span class="o">.</span><span class="n">set</span><span class="p">(</span><span class="n">xticklabels</span><span class="o">=</span><span class="p">[])</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt output_prompt">Out[7]:</div>




<div class="output_text output_subarea output_execute_result">
<pre>&lt;seaborn.axisgrid.PairGrid at 0x22cd9074048&gt;</pre>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>




<div class="output_png output_subarea ">
<img src="data:image/png;base64,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eBNQn+SIiIiIiIh40+hytftqt8PAHSF/y9kC7J33eLS2rF7JzO42s0EzOzWsIjO73MyGzWx4bGyswW5lKYqjH4qjH4pjcoqhH4qjH4qjH4qjH4qjLJvkOOf+rXb3bufcVfV/Deq2sCrrHv8bcLpz7mKCr6gOrdM5t8s51+uc6z3hhBMa7FaWojj6oTj6oTgmpxj6oTj6oTj6oTj6oThK1G9X+wcz+6mZfcTMLoy4zSgwf2bmFOCR+QWcc/udc1O1h58BnhexbhERERERkVBRfyfn14BXAGPALjP7sZn9VYPNbgfONrMzzGwt8Fbg2vkFzOykeQ/fyNHL40RERERERFYk8u/kOOcec879E/B/AHcBf92g/AzwXuB6guRlwDn3EzP7sJm9sVbs/Wb2EzP7EfB+4F0r6IOIiIiIiMgRkb5C2szOB94CbAP2A9cAf9ZoO+fcdcB1dcv+et79DwEfitFeERERERGRZUX9nZzPA18CXuOce6RRYRERERERkaw0THJqP+r5c+fc/25Ce0RERERERBJp+Jkc51wVOK725QEiIiIiIiK5FvVytRHgFjO7FijPLXTO/UMqrRIREREREVmhqEnOI7W/NmBDes0RERERERFJJlKS45z727QbIiIiIiIi4kPUr5C+AXD1y51zr/TeIhERERERkQSiXq725/PudwElYMZ/c0RERERERJKJernaHXWLbjGzm1Joj4iIiIiISCJRL1c7dt7DNqAXeGYqLRIREREREUkg6uVqd3D0MzkzwMPA76fRIBERERERkSSWTXLM7PnAXufcGbXH7yT4PM7DwL2pt05ERERERCSmtgbr+4FpADN7GfAx4CrgKWBXuk0TERERERGJr9Hlau3OufHa/bcAu5xzQ8CQmd2VbtNERERERETiazST025mc4nQq4DvzFsX9fM8IiIiIiIiTdMoUfkScJOZ7QMOA98FMLOzCC5ZExERERERyZVlkxzn3EfN7NvAScA3nXNz37DWBrwv7caJiIiIiIjE1ehyNZxzu51zX3POlecte8A5d2ejbc1sq5ndb2YPmtkHQ9Z3mtmXa+t/YGanx+2AiIiIiIjIfA2TnJUys3bgU8DrgAuAt5nZBXXFfh844Jw7C/hH4O/Sao/4VS43LpPn7fMkq760YgzjtDmNsmntv5katSvpeoDJyeR1NCpTqTSuo9nSPD6K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YiVvdo6DD5ynv1rn/vo8ff/TMUeNP3XB/T/bv3UY9x7yEH4zeyR37RxiiCrfO+xEDn7OE1izBiqV4Aq7xqNeX/x+ehp+9KNgOsCjHgWPeAQ89KHw/OenF85Wsj7GJJGLuplzhevJMbNtQPOdVocC2zPITi80l227c+6MbhMN41glv3FLe536jGO3d/5lVX99fG+v41jWbX3AOXdKNwmUMIZJ85mHbTqL2Pr+zqzjmLf62Wl+0opj3uLTiSRl8BXHHUDZRlvoeRyLpnCNnChmNumcW5t1PtKQZtnyHLc85823rMpaxBgXMc9x9LJcRYlhUfK5UBZ5LmKcWslbeZQf/7Sd+FHGMvmWdHQ1ERERERGRXFMjR0RERERESqUsjZwLss5AitIsW57jlue8+ZZVWYsY4yLmOY5elqsoMSxKPhfKIs9FjFMreSuP8uOfthM/ylgmr0pxT46IiIiIiEhDWXpyREREREREADVyRERERESkZNTIERERERGRUlEjR0RERERESkWNHBERERERKRU1ckREREREpFTUyBERERERkVJRI0dEREREREpFjRwRERERESkVNXJERERERKRU1MgREREREZFSUSNHRERERERKRY0cEREREREpFTVyRERERESkVNTIERERERGRUilcI+eMM85wwHJ+eKE4+qE4+qE4dk8x9ENx9ENx9ENx9ENxXJ4K18jZvn171lkoBcXRD8XRD8Wxe4qhH4qjH4qjH4qjH4rj8lS4Ro6IiIiISFZuvRV++MOscyHtlKKRU61mnYMU5bRw813mK6fFysbMTCZf2+06zLs91Vrseeeqs7HmSxKzWvyv76l2ZZiNFwqJkGSdJ9386tX4+4kib9u+8u4rnSJsD81lrUfsz2ozOd0hFdQ//zOsW5d1LqSdwjdypqZgbCx4Lp2cFs5NTdE3NobrMF85LVY2pqZgw4aeB6PbdZh39alpVo6toz413XZeN7WFFWPrcVNb2swXP2bT08EBcLr91/dUuzJsmazyUwAAIABJREFU2QLr1wfPkkySdZ50H+impqiMbYhV94q8bfvKu690irA9NJfVTW2h0rQ/q09N078h3v5QpEwK3cipVmFiAjZtCp4L/OPVvnJauPlqFQvzZRMTiX8ty2mxsjEzszgYPerR6XYd5t2eao3KxDhs2kRlYrxlj85cdRab2BjGYiO1JXp0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
">
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Ensembling-&amp;-Stacking-models">Ensembling &amp; Stacking models<a class="anchor-link" href="#Ensembling-&amp;-Stacking-models">¶</a></h2><p>간단하게 feature eigineering을 수행해본 후 우리는 마침내 이 노트북의 핵심에 도달했습니다. 이제 스태킹 앙상블을 만들어 봅시다!</p>

</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Helpers-via-Python-Classes">Helpers via Python Classes<a class="anchor-link" href="#Helpers-via-Python-Classes">¶</a></h3><p>여기서부터는 파이썬 클래스를 활용해 보다 편리한 작업을 할 수 있도록 하겠습니다. 프로그래밍을 처음 시작하는 사람은 일반적으로 객체 지향 프로그래밍(Object Oriented Programming, OOP)과 함께 사용되는 클래스라는 개념에 대해 들어보았을 것입니다. 클래스는 객체 생성을 위한 코드/프로그램이며, 해당 클래스에 대한 특정한 함수 및 메소드를 구현하는 데 도움을 줍니다.</p>
<p><i>(* 역자 주: '클래스'는 '자동차'에, '메소드 또는 함수'는 '자동차를 구성하는 다양한 기능'들이라고 묘사할 수 있습니다. 자동차라는 클래스는 핸들 조향 기능, 가속 기능, 정지 기능과 같은 다양한 함수들을 포함하고 있습니다. 또한 자동차 클래스를 오토바이라는 새로운 클래스에 상속하면 이제 오토바이 클래스는 조향 기능, 가속 기능, 정지 기능을 상속받아 사용할 수 있게 됩니다.)</i></p>
<p>아래의 코드 섹션에서는 Sklearn이 제공하는 모든 분류기들에 공통된 inbuilt methods(train, predict, fit과 같이 분류기가 내장하고 있는 함수들)를 확장할 수 있는 SklearnHelper 클래스를 작성할 것입니다. 따라서 5개의 다른 분류기를 호출하기 위해 동일한 메소드를 5번 반복해서 작성할 필요가 없으므로 중복성이 없어집니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[8]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Some useful parameters which will come in handy later on</span>
<span class="n">ntrain</span> <span class="o">=</span> <span class="n">train</span><span class="o">.</span><span class="n">shape</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span>
<span class="n">ntest</span> <span class="o">=</span> <span class="n">test</span><span class="o">.</span><span class="n">shape</span><span class="p">[</span><span class="mi">0</span><span class="p">]</span>
<span class="n">SEED</span> <span class="o">=</span> <span class="mi">0</span> <span class="c1"># for reproducibility</span>
<span class="n">NFOLDS</span> <span class="o">=</span> <span class="mi">5</span> <span class="c1"># set folds for out-of-fold prediction</span>
<span class="n">kf</span> <span class="o">=</span> <span class="n">KFold</span><span class="p">(</span><span class="n">n_splits</span><span class="o">=</span><span class="n">NFOLDS</span><span class="p">,</span> <span class="n">random_state</span><span class="o">=</span><span class="n">SEED</span><span class="p">)</span>

<span class="c1"># Class to extend the Sklearn classifier</span>
<span class="k">class</span> <span class="nc">SklearnHelper</span><span class="p">(</span><span class="nb">object</span><span class="p">):</span>
    <span class="k">def</span> <span class="nf">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">clf</span><span class="p">,</span> <span class="n">seed</span><span class="o">=</span><span class="mi">0</span><span class="p">,</span> <span class="n">params</span><span class="o">=</span><span class="kc">None</span><span class="p">):</span>
        <span class="n">params</span><span class="p">[</span><span class="s1">'random_state'</span><span class="p">]</span> <span class="o">=</span> <span class="n">seed</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">clf</span> <span class="o">=</span> <span class="n">clf</span><span class="p">(</span><span class="o">**</span><span class="n">params</span><span class="p">)</span>

    <span class="k">def</span> <span class="nf">train</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">x_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">clf</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">x_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">)</span>

    <span class="k">def</span> <span class="nf">predict</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">x</span><span class="p">):</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">clf</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">x</span><span class="p">)</span>
    
    <span class="k">def</span> <span class="nf">fit</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span><span class="n">x</span><span class="p">,</span><span class="n">y</span><span class="p">):</span>
        <span class="k">return</span> <span class="bp">self</span><span class="o">.</span><span class="n">clf</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">x</span><span class="p">,</span><span class="n">y</span><span class="p">)</span>
    
    <span class="k">def</span> <span class="nf">feature_importances</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span><span class="n">x</span><span class="p">,</span><span class="n">y</span><span class="p">):</span>
        <span class="nb">print</span><span class="p">(</span><span class="bp">self</span><span class="o">.</span><span class="n">clf</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">x</span><span class="p">,</span><span class="n">y</span><span class="p">)</span><span class="o">.</span><span class="n">feature_importances_</span><span class="p">)</span>
    
<span class="c1"># Class to extend XGboost classifer</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>여기서는 여러분께서 이미 알고 있더라도 파이썬에서 객체나 클래스를 사용하는 게 익숙하지 않은 분들을 위해 양해를 부탁드리겠습니다. 위에서 타이핑한 코드에 대해 설명해 드리겠습니다. 기본 분류기를 만들 때 Sklearn 라이브러리에 이미 들어 있는 모델만 사용할 것이므로 우리는 이것을 클래스로 확장할 수 있습니다.</p>
<p><b>def init</b> : 클래스의 기본 생성자를 호출하는 파이썬 표준입니다. 즉 객체(여기서는 분류기가 객체입니다)를 만들기 위해서는 clf(원하는 sklearn 분류기), seed(랜덤값을 만들기 위한 초기값) 및 params(분류기 학습을 위한 파라미터)를 지정해야 합니다.</p>
<p>나머지 코드는 단순히 sklearn 분류기들에 존재하는 train 메소드를 호출하는 SklearnHelper 클래스의 메소드들입니다. 기본적으로 다양한 Sklearn 분류기들을 확장하는 래퍼(참조) 클래스를 만들어 스태커(Stacking 앙상블을 위한 객체)에 여러 학습 모델들을 구현해 쌓을 때 동일한 코드를 반복 작성해야 하는 부담을 줄여 줍니다.</p>
<h3 id="Out-of-Fold-Predictions">Out-of-Fold Predictions<a class="anchor-link" href="#Out-of-Fold-Predictions">¶</a></h3><p>소개 섹션에서 언급한 것처럼, 스태킹은 기본 분류기의 예측을 두 번째 레벨 모델의 학습을 위한 입력으로 사용합니다. 그러나 첫 번째 레벨의 모델을 학습할 때 단순히 전체 학습 데이터를 사용하여 학습을 진행하면 안됩니다. 모델이 전체 테스트셋에 대한 예측값을 만들고 그것을 두 번째 레벨 학습에 사용할 수 있기 때문입니다. 이렇게 하면 첫 번째 단계 모델이 테스트셋을 이미 "보았기" 때문에 예측 결과에 위험이 따르게 됩니다. 따라서 이러한 결과물을 이용해 두 번째 레벨 모델 학습에 넣어주게 되면 오버피팅을 유발하게 됩니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[9]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">get_oof</span><span class="p">(</span><span class="n">clf</span><span class="p">,</span> <span class="n">x_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">,</span> <span class="n">x_test</span><span class="p">):</span>
    <span class="n">oof_train</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="n">ntrain</span><span class="p">,))</span>
    <span class="n">oof_test</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros</span><span class="p">((</span><span class="n">ntest</span><span class="p">,))</span>
    <span class="n">oof_test_skf</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">empty</span><span class="p">((</span><span class="n">NFOLDS</span><span class="p">,</span> <span class="n">ntest</span><span class="p">))</span>

    <span class="k">for</span> <span class="n">i</span><span class="p">,</span> <span class="p">(</span><span class="n">train_index</span><span class="p">,</span> <span class="n">test_index</span><span class="p">)</span> <span class="ow">in</span> <span class="nb">enumerate</span><span class="p">(</span><span class="n">kf</span><span class="o">.</span><span class="n">split</span><span class="p">(</span><span class="n">x_train</span><span class="p">)):</span>
        <span class="n">x_tr</span> <span class="o">=</span> <span class="n">x_train</span><span class="p">[</span><span class="n">train_index</span><span class="p">]</span>
        <span class="n">y_tr</span> <span class="o">=</span> <span class="n">y_train</span><span class="p">[</span><span class="n">train_index</span><span class="p">]</span>
        <span class="n">x_te</span> <span class="o">=</span> <span class="n">x_train</span><span class="p">[</span><span class="n">test_index</span><span class="p">]</span>

        <span class="n">clf</span><span class="o">.</span><span class="n">train</span><span class="p">(</span><span class="n">x_tr</span><span class="p">,</span> <span class="n">y_tr</span><span class="p">)</span>

        <span class="n">oof_train</span><span class="p">[</span><span class="n">test_index</span><span class="p">]</span> <span class="o">=</span> <span class="n">clf</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">x_te</span><span class="p">)</span>
        <span class="n">oof_test_skf</span><span class="p">[</span><span class="n">i</span><span class="p">,</span> <span class="p">:]</span> <span class="o">=</span> <span class="n">clf</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">x_test</span><span class="p">)</span>

    <span class="n">oof_test</span><span class="p">[:]</span> <span class="o">=</span> <span class="n">oof_test_skf</span><span class="o">.</span><span class="n">mean</span><span class="p">(</span><span class="n">axis</span><span class="o">=</span><span class="mi">0</span><span class="p">)</span>
    <span class="k">return</span> <span class="n">oof_train</span><span class="o">.</span><span class="n">reshape</span><span class="p">(</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">),</span> <span class="n">oof_test</span><span class="o">.</span><span class="n">reshape</span><span class="p">(</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="첫-번째-단계의-모델들을-생성하기">첫 번째 단계의 모델들을 생성하기<a class="anchor-link" href="#첫-번째-단계의-모델들을-생성하기">¶</a></h2><p>이제부터 다섯 가지의 분류기를 첫 번째 단계 학습을 위해 준비하겠습니다. 이 모델들은 모두 Sklearn 라이브러리를 통해 편리하게 호출할 수 있으며, 우리가 사용할 분류기의 종류는 다음과 같습니다.</p>
<ul>
<li>Random Forest Classifier</li>
<li>Extra Trees Classifier</li>
<li>AdaBoost Classifier</li>
<li>Gradient Boosting Classifier</li>
<li>Support Vector Machine</li>
</ul>
<h4 id="인자(파라미터)-소개">인자(파라미터) 소개<a class="anchor-link" href="#인자(파라미터)-소개">¶</a></h4><p>매개 변수들에 대한 간단한 요약만 말씀드리겠습니다.</p>
<ul>
<li><b>n_jobs</b> : 학습 과정에 사용될 (CPU)코어 개수입니다. -1로 설정하면 모든 코어가 사용됩니다. </li>
<li><b>n_estimators</b> : 학습 모델의 분류 트리 개수입니다. (기본값은 10개로 설정됩니다.)</li>
<li><b>max_depth</b> : 트리의 최대 깊이 또는 노드의 확장 정도를 의미합니다. 지나치게 많은 수를 설정하면 트리가 너무 깊어지며 과적합(Overfitting)할 수 있습니다. </li>
<li><b>verbose</b> : 학습 과정 중 텍스트를 출력할지 여부를 조정합니다. 값을 0으로 주면 모든 텍스트를 출력하지 않는 반면 값 3은 모든 학습 이터레이션 중 트리 학습 프로세스에 대한 텍스트를 출력합니다. </li>
</ul>
<p>Sklearn 공식 웹 사이트를 통해 보다 자세한 설명을 확인하시기 바랍니다. 다른 유용한 파라미터가 있음을 확인하실 수 있을 것입니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[10]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># 위에서 언급한 분류기들에 파라미터를 넣는 과정입니다. </span>

<span class="c1"># Random Forest parameters</span>
<span class="n">rf_params</span> <span class="o">=</span> <span class="p">{</span>
    <span class="s1">'n_jobs'</span><span class="p">:</span> <span class="o">-</span><span class="mi">1</span><span class="p">,</span>
    <span class="s1">'n_estimators'</span><span class="p">:</span> <span class="mi">500</span><span class="p">,</span>
     <span class="s1">'warm_start'</span><span class="p">:</span> <span class="kc">True</span><span class="p">,</span> 
     <span class="c1">#'max_features': 0.2,</span>
    <span class="s1">'max_depth'</span><span class="p">:</span> <span class="mi">6</span><span class="p">,</span>
    <span class="s1">'min_samples_leaf'</span><span class="p">:</span> <span class="mi">2</span><span class="p">,</span>
    <span class="s1">'max_features'</span> <span class="p">:</span> <span class="s1">'sqrt'</span><span class="p">,</span>
    <span class="s1">'verbose'</span><span class="p">:</span> <span class="mi">0</span>
<span class="p">}</span>

<span class="c1"># Extra Trees Parameters</span>
<span class="n">et_params</span> <span class="o">=</span> <span class="p">{</span>
    <span class="s1">'n_jobs'</span><span class="p">:</span> <span class="o">-</span><span class="mi">1</span><span class="p">,</span>
    <span class="s1">'n_estimators'</span><span class="p">:</span><span class="mi">500</span><span class="p">,</span>
    <span class="c1">#'max_features': 0.5,</span>
    <span class="s1">'max_depth'</span><span class="p">:</span> <span class="mi">8</span><span class="p">,</span>
    <span class="s1">'min_samples_leaf'</span><span class="p">:</span> <span class="mi">2</span><span class="p">,</span>
    <span class="s1">'verbose'</span><span class="p">:</span> <span class="mi">0</span>
<span class="p">}</span>

<span class="c1"># AdaBoost parameters</span>
<span class="n">ada_params</span> <span class="o">=</span> <span class="p">{</span>
    <span class="s1">'n_estimators'</span><span class="p">:</span> <span class="mi">500</span><span class="p">,</span>
    <span class="s1">'learning_rate'</span> <span class="p">:</span> <span class="mf">0.75</span>
<span class="p">}</span>

<span class="c1"># Gradient Boosting parameters</span>
<span class="n">gb_params</span> <span class="o">=</span> <span class="p">{</span>
    <span class="s1">'n_estimators'</span><span class="p">:</span> <span class="mi">500</span><span class="p">,</span>
     <span class="c1">#'max_features': 0.2,</span>
    <span class="s1">'max_depth'</span><span class="p">:</span> <span class="mi">5</span><span class="p">,</span>
    <span class="s1">'min_samples_leaf'</span><span class="p">:</span> <span class="mi">2</span><span class="p">,</span>
    <span class="s1">'verbose'</span><span class="p">:</span> <span class="mi">0</span>
<span class="p">}</span>

<span class="c1"># Support Vector Classifier parameters </span>
<span class="n">svc_params</span> <span class="o">=</span> <span class="p">{</span>
    <span class="s1">'kernel'</span> <span class="p">:</span> <span class="s1">'linear'</span><span class="p">,</span>
    <span class="s1">'C'</span> <span class="p">:</span> <span class="mf">0.025</span>
    <span class="p">}</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>위에서 객체지향 프로그래밍의 프레임워크 내에서 객체와 클래스에 대해 언급하였듯, 앞서 정의한 Helper Sklearn 클래스를 통해 다섯 가지의 분류기를 담고있는 5개의 객체를 생성해 보겠습니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[11]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># 다섯 개의 분류기를 담고 있는 다섯 개의 객체 생성하기</span>
<span class="n">rf</span> <span class="o">=</span> <span class="n">SklearnHelper</span><span class="p">(</span><span class="n">clf</span><span class="o">=</span><span class="n">RandomForestClassifier</span><span class="p">,</span> <span class="n">seed</span><span class="o">=</span><span class="n">SEED</span><span class="p">,</span> <span class="n">params</span><span class="o">=</span><span class="n">rf_params</span><span class="p">)</span>
<span class="n">et</span> <span class="o">=</span> <span class="n">SklearnHelper</span><span class="p">(</span><span class="n">clf</span><span class="o">=</span><span class="n">ExtraTreesClassifier</span><span class="p">,</span> <span class="n">seed</span><span class="o">=</span><span class="n">SEED</span><span class="p">,</span> <span class="n">params</span><span class="o">=</span><span class="n">et_params</span><span class="p">)</span>
<span class="n">ada</span> <span class="o">=</span> <span class="n">SklearnHelper</span><span class="p">(</span><span class="n">clf</span><span class="o">=</span><span class="n">AdaBoostClassifier</span><span class="p">,</span> <span class="n">seed</span><span class="o">=</span><span class="n">SEED</span><span class="p">,</span> <span class="n">params</span><span class="o">=</span><span class="n">ada_params</span><span class="p">)</span>
<span class="n">gb</span> <span class="o">=</span> <span class="n">SklearnHelper</span><span class="p">(</span><span class="n">clf</span><span class="o">=</span><span class="n">GradientBoostingClassifier</span><span class="p">,</span> <span class="n">seed</span><span class="o">=</span><span class="n">SEED</span><span class="p">,</span> <span class="n">params</span><span class="o">=</span><span class="n">gb_params</span><span class="p">)</span>
<span class="n">svc</span> <span class="o">=</span> <span class="n">SklearnHelper</span><span class="p">(</span><span class="n">clf</span><span class="o">=</span><span class="n">SVC</span><span class="p">,</span> <span class="n">seed</span><span class="o">=</span><span class="n">SEED</span><span class="p">,</span> <span class="n">params</span><span class="o">=</span><span class="n">svc_params</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h4 id="Train-데이터와-Test-데이터로부터-Numpy-배열-만들기">Train 데이터와 Test 데이터로부터 Numpy 배열 만들기<a class="anchor-link" href="#Train-데이터와-Test-데이터로부터-Numpy-배열-만들기">¶</a></h4><p>좋습니다. 첫 번째 레벨의 기본 모델 객체를 준비한 후 다음과 같이 원래의 판다스 데이터프레임에서 Numpy 배열을 생성해 학습을 위한 Train과 Test 데이터를 준비할 수 있습니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[12]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Create Numpy arrays of train, test and target ( Survived) dataframes to feed into our models</span>
<span class="n">y_train</span> <span class="o">=</span> <span class="n">train</span><span class="p">[</span><span class="s1">'Survived'</span><span class="p">]</span><span class="o">.</span><span class="n">ravel</span><span class="p">()</span>
<span class="n">train</span> <span class="o">=</span> <span class="n">train</span><span class="o">.</span><span class="n">drop</span><span class="p">([</span><span class="s1">'Survived'</span><span class="p">],</span> <span class="n">axis</span><span class="o">=</span><span class="mi">1</span><span class="p">)</span>
<span class="n">x_train</span> <span class="o">=</span> <span class="n">train</span><span class="o">.</span><span class="n">values</span> <span class="c1"># Creates an array of the train data</span>
<span class="n">x_test</span> <span class="o">=</span> <span class="n">test</span><span class="o">.</span><span class="n">values</span> <span class="c1"># Creats an array of the test data</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h4 id="첫-번째-레벨의-예측값-구하기">첫 번째 레벨의 예측값 구하기<a class="anchor-link" href="#첫-번째-레벨의-예측값-구하기">¶</a></h4><p>이제 다섯 개의 첫 번째 레벨 분류기에 Train과 Test 데이터를 넣어 주고 앞서 정의한 Out-of-Fold 예측 함수를 사용해 첫 번째 레벨의 예측값을 구해 봅니다. 아래 코드는 실행에 시간이 다소 걸리므로 몇 분 정도 기다려 주십시오.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[13]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Out-of-Fold train과 test 예측값을 만듭니다. 이 과정을 통해 생성된 예측값은 새로운 feature로서 사용됩니다. </span>
<span class="n">et_oof_train</span><span class="p">,</span> <span class="n">et_oof_test</span> <span class="o">=</span> <span class="n">get_oof</span><span class="p">(</span><span class="n">et</span><span class="p">,</span> <span class="n">x_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">,</span> <span class="n">x_test</span><span class="p">)</span> <span class="c1"># Extra Trees</span>
<span class="n">rf_oof_train</span><span class="p">,</span> <span class="n">rf_oof_test</span> <span class="o">=</span> <span class="n">get_oof</span><span class="p">(</span><span class="n">rf</span><span class="p">,</span><span class="n">x_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">,</span> <span class="n">x_test</span><span class="p">)</span> <span class="c1"># Random Forest</span>
<span class="n">ada_oof_train</span><span class="p">,</span> <span class="n">ada_oof_test</span> <span class="o">=</span> <span class="n">get_oof</span><span class="p">(</span><span class="n">ada</span><span class="p">,</span> <span class="n">x_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">,</span> <span class="n">x_test</span><span class="p">)</span> <span class="c1"># AdaBoost </span>
<span class="n">gb_oof_train</span><span class="p">,</span> <span class="n">gb_oof_test</span> <span class="o">=</span> <span class="n">get_oof</span><span class="p">(</span><span class="n">gb</span><span class="p">,</span><span class="n">x_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">,</span> <span class="n">x_test</span><span class="p">)</span> <span class="c1"># Gradient Boost</span>
<span class="n">svc_oof_train</span><span class="p">,</span> <span class="n">svc_oof_test</span> <span class="o">=</span> <span class="n">get_oof</span><span class="p">(</span><span class="n">svc</span><span class="p">,</span><span class="n">x_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">,</span> <span class="n">x_test</span><span class="p">)</span> <span class="c1"># Support Vector Classifier</span>

<span class="nb">print</span><span class="p">(</span><span class="s2">"Training is complete"</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>Training is complete
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h4 id="다양한-분류기로부터-생성된-Feature-Importance">다양한 분류기로부터 생성된 Feature Importance<a class="anchor-link" href="#다양한-분류기로부터-생성된-Feature-Importance">¶</a></h4><p>이제 첫 번째 레벨 분류기들의 학습을 마쳤으므로 Sklearn 모델들의 매우 멋진 기능을 활용할 수 있습니다. 이는 매우 간단한 코드 한 줄만으로 Train 및 Test 세트에서 다양한 features를 출력하는 것입니다.</p>
<p>Sklearn 공식 문서에 따르면 대부분의 분류기들은 단순히 .featureimportances를 입력하는것 만으로도 feature importance를 반환하도록 작성되었습니다. 그러므로 우리는 함수 earliand를 사용하여 매우 유용한 feature를 뽑고 그려 볼 것입니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[14]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">rf_feature</span> <span class="o">=</span> <span class="n">rf</span><span class="o">.</span><span class="n">feature_importances</span><span class="p">(</span><span class="n">x_train</span><span class="p">,</span><span class="n">y_train</span><span class="p">)</span>
<span class="n">et_feature</span> <span class="o">=</span> <span class="n">et</span><span class="o">.</span><span class="n">feature_importances</span><span class="p">(</span><span class="n">x_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">)</span>
<span class="n">ada_feature</span> <span class="o">=</span> <span class="n">ada</span><span class="o">.</span><span class="n">feature_importances</span><span class="p">(</span><span class="n">x_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">)</span>
<span class="n">gb_feature</span> <span class="o">=</span> <span class="n">gb</span><span class="o">.</span><span class="n">feature_importances</span><span class="p">(</span><span class="n">x_train</span><span class="p">,</span><span class="n">y_train</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>


<div class="output_subarea output_stream output_stdout output_text">
<pre>[0.12574505 0.20066884 0.0299172  0.02151793 0.07240045 0.02311772
 0.10927758 0.06431927 0.06812475 0.01325286 0.27165836]
[0.11886245 0.38123302 0.02786382 0.01651714 0.05557273 0.02859385
 0.04663404 0.08283731 0.04629938 0.02204419 0.17354208]
[0.03  0.008 0.02  0.062 0.038 0.01  0.698 0.014 0.05  0.006 0.064]
[0.08687776 0.00949799 0.04991072 0.01443587 0.05850666 0.02334782
 0.1690867  0.03866105 0.11246703 0.00426682 0.43294159]
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>우리는 아직 feature importance를 할당하고 저장하는 방법을 알지 못합니다. 따라서 위의 코드에서 예측값을 출력하고 아래에서처럼 파이썬 List 자료형에 복사할 것입니다. (지루한 팁에 대해서 양해 부탁드립니다.)</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[15]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">rf_features</span> <span class="o">=</span> <span class="p">[</span><span class="mf">0.10474135</span><span class="p">,</span>  <span class="mf">0.21837029</span><span class="p">,</span>  <span class="mf">0.04432652</span><span class="p">,</span>  <span class="mf">0.02249159</span><span class="p">,</span>  <span class="mf">0.05432591</span><span class="p">,</span>  <span class="mf">0.02854371</span>
  <span class="p">,</span><span class="mf">0.07570305</span><span class="p">,</span>  <span class="mf">0.01088129</span> <span class="p">,</span> <span class="mf">0.24247496</span><span class="p">,</span>  <span class="mf">0.13685733</span> <span class="p">,</span> <span class="mf">0.06128402</span><span class="p">]</span>
<span class="n">et_features</span> <span class="o">=</span> <span class="p">[</span> <span class="mf">0.12165657</span><span class="p">,</span>  <span class="mf">0.37098307</span>  <span class="p">,</span><span class="mf">0.03129623</span> <span class="p">,</span> <span class="mf">0.01591611</span> <span class="p">,</span> <span class="mf">0.05525811</span> <span class="p">,</span> <span class="mf">0.028157</span>
  <span class="p">,</span><span class="mf">0.04589793</span> <span class="p">,</span> <span class="mf">0.02030357</span> <span class="p">,</span> <span class="mf">0.17289562</span> <span class="p">,</span> <span class="mf">0.04853517</span><span class="p">,</span>  <span class="mf">0.08910063</span><span class="p">]</span>
<span class="n">ada_features</span> <span class="o">=</span> <span class="p">[</span><span class="mf">0.028</span> <span class="p">,</span>   <span class="mf">0.008</span>  <span class="p">,</span>      <span class="mf">0.012</span>   <span class="p">,</span>     <span class="mf">0.05866667</span><span class="p">,</span>   <span class="mf">0.032</span> <span class="p">,</span>       <span class="mf">0.008</span>
  <span class="p">,</span><span class="mf">0.04666667</span> <span class="p">,</span>  <span class="mf">0.</span>     <span class="p">,</span>      <span class="mf">0.05733333</span><span class="p">,</span>   <span class="mf">0.73866667</span><span class="p">,</span>   <span class="mf">0.01066667</span><span class="p">]</span>
<span class="n">gb_features</span> <span class="o">=</span> <span class="p">[</span> <span class="mf">0.06796144</span> <span class="p">,</span> <span class="mf">0.03889349</span> <span class="p">,</span> <span class="mf">0.07237845</span> <span class="p">,</span> <span class="mf">0.02628645</span> <span class="p">,</span> <span class="mf">0.11194395</span><span class="p">,</span>  <span class="mf">0.04778854</span>
  <span class="p">,</span><span class="mf">0.05965792</span> <span class="p">,</span> <span class="mf">0.02774745</span><span class="p">,</span>  <span class="mf">0.07462718</span><span class="p">,</span>  <span class="mf">0.4593142</span> <span class="p">,</span>  <span class="mf">0.01340093</span><span class="p">]</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Ploty 패키지를 통해 쉽게 그래프를 그릴 수 있도록 feature importance가 들어 있는 List로부터 판다스 데이터프레임을 만듭니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[16]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">cols</span> <span class="o">=</span> <span class="n">train</span><span class="o">.</span><span class="n">columns</span><span class="o">.</span><span class="n">values</span>
<span class="c1"># Create a dataframe with features</span>
<span class="n">feature_dataframe</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">DataFrame</span><span class="p">(</span> <span class="p">{</span><span class="s1">'features'</span><span class="p">:</span> <span class="n">cols</span><span class="p">,</span>
     <span class="s1">'Random Forest feature importances'</span><span class="p">:</span> <span class="n">rf_features</span><span class="p">,</span>
     <span class="s1">'Extra Trees  feature importances'</span><span class="p">:</span> <span class="n">et_features</span><span class="p">,</span>
      <span class="s1">'AdaBoost feature importances'</span><span class="p">:</span> <span class="n">ada_features</span><span class="p">,</span>
    <span class="s1">'Gradient Boost feature importances'</span><span class="p">:</span> <span class="n">gb_features</span>
    <span class="p">})</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h4 id="Ploty-Scatterplots를-통해-Interactive-feature-importance-불러오기">Ploty Scatterplots를 통해 Interactive feature importance 불러오기<a class="anchor-link" href="#Ploty-Scatterplots를-통해-Interactive-feature-importance-불러오기">¶</a></h4><p>여기서 대화형 Ploty 패키지를 사용하여 다음과 같이 'Scatter'를 호출해 여러 분류기로부터 얻은 feature importance를 ploty scatter plot으로 시각화합니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[17]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Scatter plot </span>
<span class="n">trace</span> <span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Scatter</span><span class="p">(</span>
    <span class="n">y</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'Random Forest feature importances'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
    <span class="n">x</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'features'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
    <span class="n">mode</span><span class="o">=</span><span class="s1">'markers'</span><span class="p">,</span>
    <span class="n">marker</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span>
        <span class="n">sizemode</span> <span class="o">=</span> <span class="s1">'diameter'</span><span class="p">,</span>
        <span class="n">sizeref</span> <span class="o">=</span> <span class="mi">1</span><span class="p">,</span>
        <span class="n">size</span> <span class="o">=</span> <span class="mi">25</span><span class="p">,</span>
<span class="c1">#       size= feature_dataframe['AdaBoost feature importances'].values,</span>
        <span class="c1">#color = np.random.randn(500), #set color equal to a variable</span>
        <span class="n">color</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'Random Forest feature importances'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
        <span class="n">colorscale</span><span class="o">=</span><span class="s1">'Portland'</span><span class="p">,</span>
        <span class="n">showscale</span><span class="o">=</span><span class="kc">True</span>
    <span class="p">),</span>
    <span class="n">text</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'features'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span>
<span class="p">)</span>
<span class="n">data</span> <span class="o">=</span> <span class="p">[</span><span class="n">trace</span><span class="p">]</span>

<span class="n">layout</span><span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Layout</span><span class="p">(</span>
    <span class="n">autosize</span><span class="o">=</span> <span class="kc">True</span><span class="p">,</span>
    <span class="n">title</span><span class="o">=</span> <span class="s1">'Random Forest Feature Importance'</span><span class="p">,</span>
    <span class="n">hovermode</span><span class="o">=</span> <span class="s1">'closest'</span><span class="p">,</span>
<span class="c1">#     xaxis= dict(</span>
<span class="c1">#         title= 'Pop',</span>
<span class="c1">#         ticklen= 5,</span>
<span class="c1">#         zeroline= False,</span>
<span class="c1">#         gridwidth= 2,</span>
<span class="c1">#     ),</span>
    <span class="n">yaxis</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span>
        <span class="n">title</span><span class="o">=</span> <span class="s1">'Feature Importance'</span><span class="p">,</span>
        <span class="n">ticklen</span><span class="o">=</span> <span class="mi">5</span><span class="p">,</span>
        <span class="n">gridwidth</span><span class="o">=</span> <span class="mi">2</span>
    <span class="p">),</span>
    <span class="n">showlegend</span><span class="o">=</span> <span class="kc">False</span>
<span class="p">)</span>
<span class="n">fig</span> <span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Figure</span><span class="p">(</span><span class="n">data</span><span class="o">=</span><span class="n">data</span><span class="p">,</span> <span class="n">layout</span><span class="o">=</span><span class="n">layout</span><span class="p">)</span>
<span class="n">py</span><span class="o">.</span><span class="n">iplot</span><span class="p">(</span><span class="n">fig</span><span class="p">,</span><span class="n">filename</span><span class="o">=</span><span class="s1">'scatter2010'</span><span class="p">)</span>

<span class="c1"># Scatter plot </span>
<span class="n">trace</span> <span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Scatter</span><span class="p">(</span>
    <span class="n">y</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'Extra Trees  feature importances'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
    <span class="n">x</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'features'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
    <span class="n">mode</span><span class="o">=</span><span class="s1">'markers'</span><span class="p">,</span>
    <span class="n">marker</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span>
        <span class="n">sizemode</span> <span class="o">=</span> <span class="s1">'diameter'</span><span class="p">,</span>
        <span class="n">sizeref</span> <span class="o">=</span> <span class="mi">1</span><span class="p">,</span>
        <span class="n">size</span> <span class="o">=</span> <span class="mi">25</span><span class="p">,</span>
<span class="c1">#       size= feature_dataframe['AdaBoost feature importances'].values,</span>
        <span class="c1">#color = np.random.randn(500), #set color equal to a variable</span>
        <span class="n">color</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'Extra Trees  feature importances'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
        <span class="n">colorscale</span><span class="o">=</span><span class="s1">'Portland'</span><span class="p">,</span>
        <span class="n">showscale</span><span class="o">=</span><span class="kc">True</span>
    <span class="p">),</span>
    <span class="n">text</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'features'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span>
<span class="p">)</span>
<span class="n">data</span> <span class="o">=</span> <span class="p">[</span><span class="n">trace</span><span class="p">]</span>

<span class="n">layout</span><span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Layout</span><span class="p">(</span>
    <span class="n">autosize</span><span class="o">=</span> <span class="kc">True</span><span class="p">,</span>
    <span class="n">title</span><span class="o">=</span> <span class="s1">'Extra Trees Feature Importance'</span><span class="p">,</span>
    <span class="n">hovermode</span><span class="o">=</span> <span class="s1">'closest'</span><span class="p">,</span>
<span class="c1">#     xaxis= dict(</span>
<span class="c1">#         title= 'Pop',</span>
<span class="c1">#         ticklen= 5,</span>
<span class="c1">#         zeroline= False,</span>
<span class="c1">#         gridwidth= 2,</span>
<span class="c1">#     ),</span>
    <span class="n">yaxis</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span>
        <span class="n">title</span><span class="o">=</span> <span class="s1">'Feature Importance'</span><span class="p">,</span>
        <span class="n">ticklen</span><span class="o">=</span> <span class="mi">5</span><span class="p">,</span>
        <span class="n">gridwidth</span><span class="o">=</span> <span class="mi">2</span>
    <span class="p">),</span>
    <span class="n">showlegend</span><span class="o">=</span> <span class="kc">False</span>
<span class="p">)</span>
<span class="n">fig</span> <span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Figure</span><span class="p">(</span><span class="n">data</span><span class="o">=</span><span class="n">data</span><span class="p">,</span> <span class="n">layout</span><span class="o">=</span><span class="n">layout</span><span class="p">)</span>
<span class="n">py</span><span class="o">.</span><span class="n">iplot</span><span class="p">(</span><span class="n">fig</span><span class="p">,</span><span class="n">filename</span><span class="o">=</span><span class="s1">'scatter2010'</span><span class="p">)</span>

<span class="c1"># Scatter plot </span>
<span class="n">trace</span> <span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Scatter</span><span class="p">(</span>
    <span class="n">y</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'AdaBoost feature importances'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
    <span class="n">x</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'features'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
    <span class="n">mode</span><span class="o">=</span><span class="s1">'markers'</span><span class="p">,</span>
    <span class="n">marker</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span>
        <span class="n">sizemode</span> <span class="o">=</span> <span class="s1">'diameter'</span><span class="p">,</span>
        <span class="n">sizeref</span> <span class="o">=</span> <span class="mi">1</span><span class="p">,</span>
        <span class="n">size</span> <span class="o">=</span> <span class="mi">25</span><span class="p">,</span>
<span class="c1">#       size= feature_dataframe['AdaBoost feature importances'].values,</span>
        <span class="c1">#color = np.random.randn(500), #set color equal to a variable</span>
        <span class="n">color</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'AdaBoost feature importances'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
        <span class="n">colorscale</span><span class="o">=</span><span class="s1">'Portland'</span><span class="p">,</span>
        <span class="n">showscale</span><span class="o">=</span><span class="kc">True</span>
    <span class="p">),</span>
    <span class="n">text</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'features'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span>
<span class="p">)</span>
<span class="n">data</span> <span class="o">=</span> <span class="p">[</span><span class="n">trace</span><span class="p">]</span>

<span class="n">layout</span><span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Layout</span><span class="p">(</span>
    <span class="n">autosize</span><span class="o">=</span> <span class="kc">True</span><span class="p">,</span>
    <span class="n">title</span><span class="o">=</span> <span class="s1">'AdaBoost Feature Importance'</span><span class="p">,</span>
    <span class="n">hovermode</span><span class="o">=</span> <span class="s1">'closest'</span><span class="p">,</span>
<span class="c1">#     xaxis= dict(</span>
<span class="c1">#         title= 'Pop',</span>
<span class="c1">#         ticklen= 5,</span>
<span class="c1">#         zeroline= False,</span>
<span class="c1">#         gridwidth= 2,</span>
<span class="c1">#     ),</span>
    <span class="n">yaxis</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span>
        <span class="n">title</span><span class="o">=</span> <span class="s1">'Feature Importance'</span><span class="p">,</span>
        <span class="n">ticklen</span><span class="o">=</span> <span class="mi">5</span><span class="p">,</span>
        <span class="n">gridwidth</span><span class="o">=</span> <span class="mi">2</span>
    <span class="p">),</span>
    <span class="n">showlegend</span><span class="o">=</span> <span class="kc">False</span>
<span class="p">)</span>
<span class="n">fig</span> <span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Figure</span><span class="p">(</span><span class="n">data</span><span class="o">=</span><span class="n">data</span><span class="p">,</span> <span class="n">layout</span><span class="o">=</span><span class="n">layout</span><span class="p">)</span>
<span class="n">py</span><span class="o">.</span><span class="n">iplot</span><span class="p">(</span><span class="n">fig</span><span class="p">,</span><span class="n">filename</span><span class="o">=</span><span class="s1">'scatter2010'</span><span class="p">)</span>

<span class="c1"># Scatter plot </span>
<span class="n">trace</span> <span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Scatter</span><span class="p">(</span>
    <span class="n">y</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'Gradient Boost feature importances'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
    <span class="n">x</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'features'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
    <span class="n">mode</span><span class="o">=</span><span class="s1">'markers'</span><span class="p">,</span>
    <span class="n">marker</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span>
        <span class="n">sizemode</span> <span class="o">=</span> <span class="s1">'diameter'</span><span class="p">,</span>
        <span class="n">sizeref</span> <span class="o">=</span> <span class="mi">1</span><span class="p">,</span>
        <span class="n">size</span> <span class="o">=</span> <span class="mi">25</span><span class="p">,</span>
<span class="c1">#       size= feature_dataframe['AdaBoost feature importances'].values,</span>
        <span class="c1">#color = np.random.randn(500), #set color equal to a variable</span>
        <span class="n">color</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'Gradient Boost feature importances'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
        <span class="n">colorscale</span><span class="o">=</span><span class="s1">'Portland'</span><span class="p">,</span>
        <span class="n">showscale</span><span class="o">=</span><span class="kc">True</span>
    <span class="p">),</span>
    <span class="n">text</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'features'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span>
<span class="p">)</span>
<span class="n">data</span> <span class="o">=</span> <span class="p">[</span><span class="n">trace</span><span class="p">]</span>

<span class="n">layout</span><span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Layout</span><span class="p">(</span>
    <span class="n">autosize</span><span class="o">=</span> <span class="kc">True</span><span class="p">,</span>
    <span class="n">title</span><span class="o">=</span> <span class="s1">'Gradient Boosting Feature Importance'</span><span class="p">,</span>
    <span class="n">hovermode</span><span class="o">=</span> <span class="s1">'closest'</span><span class="p">,</span>
<span class="c1">#     xaxis= dict(</span>
<span class="c1">#         title= 'Pop',</span>
<span class="c1">#         ticklen= 5,</span>
<span class="c1">#         zeroline= False,</span>
<span class="c1">#         gridwidth= 2,</span>
<span class="c1">#     ),</span>
    <span class="n">yaxis</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span>
        <span class="n">title</span><span class="o">=</span> <span class="s1">'Feature Importance'</span><span class="p">,</span>
        <span class="n">ticklen</span><span class="o">=</span> <span class="mi">5</span><span class="p">,</span>
        <span class="n">gridwidth</span><span class="o">=</span> <span class="mi">2</span>
    <span class="p">),</span>
    <span class="n">showlegend</span><span class="o">=</span> <span class="kc">False</span>
<span class="p">)</span>
<span class="n">fig</span> <span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Figure</span><span class="p">(</span><span class="n">data</span><span class="o">=</span><span class="n">data</span><span class="p">,</span> <span class="n">layout</span><span class="o">=</span><span class="n">layout</span><span class="p">)</span>
<span class="n">py</span><span class="o">.</span><span class="n">iplot</span><span class="p">(</span><span class="n">fig</span><span class="p">,</span><span class="n">filename</span><span class="o">=</span><span class="s1">'scatter2010'</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>



<div class="output_html rendered_html output_subarea ">
<div>
        
        
            <div id="38b0a184-4142-4122-be09-1d97b99a762d" class="plotly-graph-div js-plotly-plot" style="height:525px; width:100%;"><div class="plot-container plotly"><div class="svg-container" style="position: relative; width: 100%; height: 100%;"><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525" style="background: rgb(255, 255, 255);"><defs id="defs-69ac2d"><g class="clips"><clipPath id="clip69ac2dxyplot" class="plotclip"><rect width="424" height="345"></rect></clipPath><clipPath class="axesclip" id="clip69ac2dx"><rect x="80" y="0" width="424" height="525"></rect></clipPath><clipPath class="axesclip" id="clip69ac2dy"><rect x="0" y="100" width="602.844" height="345"></rect></clipPath><clipPath class="axesclip" id="clip69ac2dxy"><rect x="80" y="100" width="424" height="345"></rect></clipPath></g><g class="gradients"><linearGradient x1="0" x2="0" y1="1" y2="0" id="g69ac2d-cbbeb52a36-4a12-4642-afcd-5fb921b14099"><stop offset="0%" stop-color="rgb(12, 51, 131)" stop-opacity="1"></stop><stop offset="25%" stop-color="rgb(10, 136, 186)" stop-opacity="1"></stop><stop offset="50%" stop-color="rgb(242, 211, 56)" stop-opacity="1"></stop><stop offset="75%" stop-color="rgb(242, 143, 56)" stop-opacity="1"></stop><stop offset="100%" stop-color="rgb(217, 30, 30)" stop-opacity="1"></stop></linearGradient></g></defs><g class="bglayer"></g><g class="draglayer cursor-crosshair"><g class="xy"><rect class="nsewdrag drag" data-subplot="xy" x="80" y="100" width="424" height="345" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nwdrag drag cursor-nw-resize" data-subplot="xy" x="60" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nedrag drag cursor-ne-resize" data-subplot="xy" x="504" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="swdrag drag cursor-sw-resize" data-subplot="xy" x="60" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sedrag drag cursor-se-resize" data-subplot="xy" x="504" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ewdrag drag cursor-ew-resize" data-subplot="xy" x="122.4" y="445.5" width="339.20000000000005" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="wdrag drag cursor-w-resize" data-subplot="xy" x="80" y="445.5" width="42.400000000000006" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="edrag drag cursor-e-resize" data-subplot="xy" x="461.6" y="445.5" width="42.400000000000006" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nsdrag drag cursor-ns-resize" data-subplot="xy" x="59.5" y="134.5" width="20" height="276" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sdrag drag cursor-s-resize" data-subplot="xy" x="59.5" y="410.5" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ndrag drag cursor-n-resize" data-subplot="xy" x="59.5" y="100" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect></g></g><g class="layer-below"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="cartesianlayer"><g class="subplot xy"><g class="layer-subplot"><g class="shapelayer"></g><g class="imagelayer"></g></g><g class="gridlayer"><g class="x"><path class="xgrid crisp" transform="translate(116.82,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(151.86,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(186.89,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(221.93,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(256.96000000000004,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(292,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(327.03,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(362.07,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(397.1,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(432.14,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(467.17,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path></g><g class="y"><path class="ygrid crisp" transform="translate(0,364.96)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,304.66999999999996)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,244.38)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,184.09)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,123.8)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path></g></g><g class="zerolinelayer"><path class="yzl zl crisp" transform="translate(0,425.25)" d="M80,0h424" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path></g><path class="xlines-below"></path><path class="ylines-below"></path><g class="overlines-below"></g><g class="xaxislayer-below"></g><g class="yaxislayer-below"></g><g class="overaxes-below"></g><g class="plot" transform="translate(80, 100)" clip-path="url('#clip69ac2dxyplot')"><g class="scatterlayer mlayer"><g class="trace scatter tracebeb52a36-4a12-4642-afcd-5fb921b14099" style="stroke-miterlimit: 2; opacity: 1;"><g class="fills"></g><g class="errorbars"></g><g class="lines"></g><g class="points"><path class="point" transform="translate(36.82,198.95)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(154, 183, 105); fill-opacity: 1;"></path><path class="point" transform="translate(71.86,61.94)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(227, 77, 41); fill-opacity: 1;"></path><path class="point" transform="translate(106.89,271.8)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 100, 163); fill-opacity: 1;"></path><path class="point" transform="translate(141.93,298.13)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 68, 142); fill-opacity: 1;"></path><path class="point" transform="translate(176.96,259.74)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(10, 115, 172); fill-opacity: 1;"></path><path class="point" transform="translate(212,290.83)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 77, 148); fill-opacity: 1;"></path><path class="point" transform="translate(247.03,233.96)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(38, 145, 170); fill-opacity: 1;"></path><path class="point" transform="translate(282.07,312.13)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 51, 131); fill-opacity: 1;"></path><path class="point" transform="translate(317.1,32.88)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(217, 30, 30); fill-opacity: 1;"></path><path class="point" transform="translate(352.14,160.23)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(242, 199, 56); fill-opacity: 1;"></path><path class="point" transform="translate(387.17,251.35)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(10, 125, 179); fill-opacity: 1;"></path></g><g class="text"></g></g></g></g><g class="overplot"></g><path class="xlines-above crisp" style="fill: none;" d="M0,0"></path><path class="ylines-above crisp" style="fill: none; stroke-width: 1px; stroke: rgb(0, 0, 0); stroke-opacity: 0;" d=""></path><g class="overlines-above"></g><g class="xaxislayer-above"><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Pclass" data-math="N" transform="translate(116.82,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Pclass</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Sex" data-math="N" transform="translate(151.86,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Sex</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Age" data-math="N" transform="translate(186.89,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Age</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Parch" data-math="N" transform="translate(221.93,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Parch</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Fare" data-math="N" transform="translate(256.96000000000004,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Fare</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Embarked" data-math="N" transform="translate(292,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Embarked</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Name_length" data-math="N" transform="translate(327.03,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Name_length</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Has_Cabin" data-math="N" transform="translate(362.07,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Has_Cabin</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="FamilySize" data-math="N" transform="translate(397.1,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">FamilySize</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="IsAlone" data-math="N" transform="translate(432.14,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">IsAlone</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Title" data-math="N" transform="translate(467.17,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Title</text></g></g><g class="yaxislayer-above"><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,425.25)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,364.96)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,304.66999999999996)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,244.38)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,184.09)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,123.8)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0" data-math="N" transform="translate(0,425.25)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.05" data-math="N" transform="translate(0,364.96)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.05</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.1" data-math="N" transform="translate(0,304.66999999999996)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.1</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.15" data-math="N" transform="translate(0,244.38)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.15</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.2" data-math="N" transform="translate(0,184.09)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.2</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.25" data-math="N" transform="translate(0,123.8)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.25</text></g></g><g class="overaxes-above"></g></g></g><g class="polarlayer"></g><g class="ternarylayer"></g><g class="geolayer"></g><g class="funnelarealayer"></g><g class="pielayer"></g><g class="sunburstlayer"></g><g class="glimages"></g></svg><div class="gl-container"></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><defs id="topdefs-69ac2d"><g class="clips"></g></defs><g class="indicatorlayer"></g><g class="layer-above"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="infolayer"><g class="cbbeb52a36-4a12-4642-afcd-5fb921b14099 colorbar" transform="translate(80,100)"><rect class="cbbg" x="431.5" y="-0.5" width="78.640625" height="346" style="fill: rgb(0, 0, 0); fill-opacity: 0; stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 0;"></rect><g class="cbfills" transform="translate(0,10)"><rect class="cbfill" x="442" width="30" y="0" height="325" style="stroke: none; fill: url(&quot;#g69ac2d-cbbeb52a36-4a12-4642-afcd-5fb921b14099&quot;);"></rect></g><g class="cblines" transform="translate(0,10)"></g><g class="cbaxis crisp" transform="translate(0,-100)"><g class="ycbbeb52a36-4a12-4642-afcd-5fb921b14099tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.05" data-math="N" transform="translate(0,380.1)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.05</text></g><g class="ycbbeb52a36-4a12-4642-afcd-5fb921b14099tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.1" data-math="N" transform="translate(0,309.94)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.1</text></g><g class="ycbbeb52a36-4a12-4642-afcd-5fb921b14099tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.15" data-math="N" transform="translate(0,239.77)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.15</text></g><g class="ycbbeb52a36-4a12-4642-afcd-5fb921b14099tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.2" data-math="N" transform="translate(0,169.61)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.2</text></g></g><g class="cbtitleunshift" transform="translate(-80,-100)"><g class="cbtitle"></g></g><rect class="cboutline" x="442" y="10" width="30" height="325" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; fill: none; stroke-width: 1;"></rect></g><g class="g-gtitle"><text class="gtitle" x="301.422" y="50" text-anchor="middle" dy="0em" data-unformatted="Random Forest Feature Importance" data-math="N" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 17px; fill: rgb(68, 68, 68); opacity: 1; font-weight: normal; white-space: pre;">Random Forest Feature Importance</text></g><g class="g-xtitle"></g><g class="g-ytitle" transform="translate(-2.65625,0)"><text class="ytitle" transform="rotate(-90,42,272.5)" x="42" y="272.5" text-anchor="middle" data-unformatted="Feature Importance" data-math="N" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 14px; fill: rgb(68, 68, 68); opacity: 1; font-weight: normal; white-space: pre;">Feature Importance</text></g></g><g class="menulayer"></g><g class="zoomlayer"></g></svg><div class="modebar-container" style="position: absolute; top: 0px; right: 0px; width: 100%;"><div id="modebar-69ac2d" class="modebar modebar--hover ease-bg"><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Download plot as a png" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m500 450c-83 0-150-67-150-150 0-83 67-150 150-150 83 0 150 67 150 150 0 83-67 150-150 150z m400 150h-120c-16 0-34 13-39 29l-31 93c-6 15-23 28-40 28h-340c-16 0-34-13-39-28l-31-94c-6-15-23-28-40-28h-120c-55 0-100-45-100-100v-450c0-55 45-100 100-100h800c55 0 100 45 100 100v450c0 55-45 100-100 100z m-400-550c-138 0-250 112-250 250 0 138 112 250 250 250 138 0 250-112 250-250 0-138-112-250-250-250z m365 380c-19 0-35 16-35 35 0 19 16 35 35 35 19 0 35-16 35-35 0-19-16-35-35-35z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn active" data-title="Zoom" data-attr="dragmode" data-val="zoom" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000-25l-250 251c40 63 63 138 63 218 0 224-182 406-407 406-224 0-406-182-406-406s183-406 407-406c80 0 155 22 218 62l250-250 125 125z m-812 250l0 438 437 0 0-438-437 0z m62 375l313 0 0-312-313 0 0 312z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Pan" data-attr="dragmode" data-val="pan" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000 350l-187 188 0-125-250 0 0 250 125 0-188 187-187-187 125 0 0-250-250 0 0 125-188-188 186-187 0 125 252 0 0-250-125 0 187-188 188 188-125 0 0 250 250 0 0-126 187 188z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Box Select" data-attr="dragmode" data-val="select" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m0 850l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m285 0l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m-857-286l0-143 143 0 0 143-143 0z m857 0l0-143 143 0 0 143-143 0z m-857-285l0-143 143 0 0 143-143 0z m857 0l0-143 143 0 0 143-143 0z m-857-286l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m285 0l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Lasso Select" data-attr="dragmode" data-val="lasso" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1031 1000" class="icon" height="1em" width="1em"><path d="m1018 538c-36 207-290 336-568 286-277-48-473-256-436-463 10-57 36-108 76-151-13-66 11-137 68-183 34-28 75-41 114-42l-55-70 0 0c-2-1-3-2-4-3-10-14-8-34 5-45 14-11 34-8 45 4 1 1 2 3 2 5l0 0 113 140c16 11 31 24 45 40 4 3 6 7 8 11 48-3 100 0 151 9 278 48 473 255 436 462z m-624-379c-80 14-149 48-197 96 42 42 109 47 156 9 33-26 47-66 41-105z m-187-74c-19 16-33 37-39 60 50-32 109-55 174-68-42-25-95-24-135 8z m360 75c-34-7-69-9-102-8 8 62-16 128-68 170-73 59-175 54-244-5-9 20-16 40-20 61-28 159 121 317 333 354s407-60 434-217c28-159-121-318-333-355z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Zoom in" data-attr="zoom" data-val="in" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m1 787l0-875 875 0 0 875-875 0z m687-500l-187 0 0-187-125 0 0 187-188 0 0 125 188 0 0 187 125 0 0-187 187 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Zoom out" data-attr="zoom" data-val="out" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m0 788l0-876 875 0 0 876-875 0z m688-500l-500 0 0 125 500 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Autoscale" data-attr="zoom" data-val="auto" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m250 850l-187 0-63 0 0-62 0-188 63 0 0 188 187 0 0 62z m688 0l-188 0 0-62 188 0 0-188 62 0 0 188 0 62-62 0z m-875-938l0 188-63 0 0-188 0-62 63 0 187 0 0 62-187 0z m875 188l0-188-188 0 0-62 188 0 62 0 0 62 0 188-62 0z m-125 188l-1 0-93-94-156 156 156 156 92-93 2 0 0 250-250 0 0-2 93-92-156-156-156 156 94 92 0 2-250 0 0-250 0 0 93 93 157-156-157-156-93 94 0 0 0-250 250 0 0 0-94 93 156 157 156-157-93-93 0 0 250 0 0 250z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Reset axes" data-attr="zoom" data-val="reset" data-toggle="false" data-gravity="n"><svg viewBox="0 0 928.6 1000" class="icon" height="1em" width="1em"><path d="m786 296v-267q0-15-11-26t-25-10h-214v214h-143v-214h-214q-15 0-25 10t-11 26v267q0 1 0 2t0 2l321 264 321-264q1-1 1-4z m124 39l-34-41q-5-5-12-6h-2q-7 0-12 3l-386 322-386-322q-7-4-13-4-7 2-12 7l-35 41q-4 5-3 13t6 12l401 334q18 15 42 15t43-15l136-114v109q0 8 5 13t13 5h107q8 0 13-5t5-13v-227l122-102q5-5 6-12t-4-13z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Toggle Spike Lines" data-attr="_cartesianSpikesEnabled" data-val="on" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="M512 409c0-57-46-104-103-104-57 0-104 47-104 104 0 57 47 103 104 103 57 0 103-46 103-103z m-327-39l92 0 0 92-92 0z m-185 0l92 0 0 92-92 0z m370-186l92 0 0 93-92 0z m0-184l92 0 0 92-92 0z" transform="matrix(1.5 0 0 -1.5 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn active" data-title="Show closest data on hover" data-attr="hovermode" data-val="closest" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1500 1000" class="icon" height="1em" width="1em"><path d="m375 725l0 0-375-375 375-374 0-1 1125 0 0 750-1125 0z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Compare data on hover" data-attr="hovermode" data-val="x" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1125 1000" class="icon" height="1em" width="1em"><path d="m187 786l0 2-187-188 188-187 0 0 937 0 0 373-938 0z m0-499l0 1-187-188 188-188 0 0 937 0 0 376-938-1z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a href="https://plot.ly/" target="_blank" data-title="Produced with Plotly" class="modebar-btn plotlyjsicon modebar-btn--logo"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 132 132" height="1em" width="1em"><defs><style>.cls-1 {fill: #119dff;} .cls-2 {fill: #25fefd;} .cls-3 {fill: #fff;}</style></defs><title>plotly-logomark</title><g id="symbol"><rect class="cls-1" width="132" height="132" rx="6" ry="6"></rect><circle class="cls-2" cx="78" cy="54" r="6"></circle><circle class="cls-2" cx="102" cy="30" r="6"></circle><circle class="cls-2" cx="78" cy="30" r="6"></circle><circle class="cls-2" cx="54" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="54" r="6"></circle><path class="cls-3" d="M30,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,30,72Z"></path><path class="cls-3" d="M78,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,78,72Z"></path><path class="cls-3" d="M54,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,54,48Z"></path><path class="cls-3" d="M102,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,102,48Z"></path></g></svg></a></div></div></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><g class="hoverlayer"></g></svg></div></div></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';
                    
                if (document.getElementById("38b0a184-4142-4122-be09-1d97b99a762d")) {
                    Plotly.newPlot(
                        '38b0a184-4142-4122-be09-1d97b99a762d',
                        [{"marker": {"color": [0.10474135, 0.21837029, 0.04432652, 0.02249159, 0.05432591, 0.02854371, 0.07570305, 0.01088129, 0.24247496, 0.13685733, 0.06128402], "colorscale": "Portland", "showscale": true, "size": 25, "sizemode": "diameter", "sizeref": 1}, "mode": "markers", "text": ["Pclass", "Sex", "Age", "Parch", "Fare", "Embarked", "Name_length", "Has_Cabin", "FamilySize", "IsAlone", "Title"], "type": "scatter", "uid": "beb52a36-4a12-4642-afcd-5fb921b14099", "x": ["Pclass", "Sex", "Age", "Parch", "Fare", "Embarked", "Name_length", "Has_Cabin", "FamilySize", "IsAlone", "Title"], "y": [0.10474135, 0.21837029, 0.04432652, 0.02249159, 0.05432591, 0.02854371, 0.07570305, 0.01088129, 0.24247496, 0.13685733, 0.06128402]}],
                        {"autosize": true, "hovermode": "closest", "showlegend": false, "title": {"text": "Random Forest Feature Importance"}, "yaxis": {"gridwidth": 2, "ticklen": 5, "title": {"text": "Feature Importance"}}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){
                            
var gd = document.getElementById('38b0a184-4142-4122-be09-1d97b99a762d');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>



<div class="output_html rendered_html output_subarea ">
<div>
        
        
            <div id="8f074751-16da-4a03-aab5-567c726922d0" class="plotly-graph-div js-plotly-plot" style="height:525px; width:100%;"><div class="plot-container plotly"><div class="svg-container" style="position: relative; width: 100%; height: 100%;"><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525" style="background: rgb(255, 255, 255);"><defs id="defs-fa0e2b"><g class="clips"><clipPath id="clipfa0e2bxyplot" class="plotclip"><rect width="424" height="345"></rect></clipPath><clipPath class="axesclip" id="clipfa0e2bx"><rect x="80" y="0" width="424" height="525"></rect></clipPath><clipPath class="axesclip" id="clipfa0e2by"><rect x="0" y="100" width="602.844" height="345"></rect></clipPath><clipPath class="axesclip" id="clipfa0e2bxy"><rect x="80" y="100" width="424" height="345"></rect></clipPath></g><g class="gradients"><linearGradient x1="0" x2="0" y1="1" y2="0" id="gfa0e2b-cb2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2"><stop offset="0%" stop-color="rgb(12, 51, 131)" stop-opacity="1"></stop><stop offset="25%" stop-color="rgb(10, 136, 186)" stop-opacity="1"></stop><stop offset="50%" stop-color="rgb(242, 211, 56)" stop-opacity="1"></stop><stop offset="75%" stop-color="rgb(242, 143, 56)" stop-opacity="1"></stop><stop offset="100%" stop-color="rgb(217, 30, 30)" stop-opacity="1"></stop></linearGradient></g></defs><g class="bglayer"></g><g class="draglayer cursor-crosshair"><g class="xy"><rect class="nsewdrag drag" data-subplot="xy" x="80" y="100" width="424" height="345" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nwdrag drag cursor-nw-resize" data-subplot="xy" x="60" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nedrag drag cursor-ne-resize" data-subplot="xy" x="504" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="swdrag drag cursor-sw-resize" data-subplot="xy" x="60" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sedrag drag cursor-se-resize" data-subplot="xy" x="504" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ewdrag drag cursor-ew-resize" data-subplot="xy" x="122.4" y="445.5" width="339.20000000000005" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="wdrag drag cursor-w-resize" data-subplot="xy" x="80" y="445.5" width="42.400000000000006" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="edrag drag cursor-e-resize" data-subplot="xy" x="461.6" y="445.5" width="42.400000000000006" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nsdrag drag cursor-ns-resize" data-subplot="xy" x="59.5" y="134.5" width="20" height="276" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sdrag drag cursor-s-resize" data-subplot="xy" x="59.5" y="410.5" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ndrag drag cursor-n-resize" data-subplot="xy" x="59.5" y="100" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect></g></g><g class="layer-below"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="cartesianlayer"><g class="subplot xy"><g class="layer-subplot"><g class="shapelayer"></g><g class="imagelayer"></g></g><g class="gridlayer"><g class="x"><path class="xgrid crisp" transform="translate(116.82,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(151.86,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(186.89,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(221.93,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(256.96000000000004,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(292,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(327.03,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(362.07,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(397.1,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(432.14,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(467.17,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path></g><g class="y"><path class="ygrid crisp" transform="translate(0,385.32)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,346)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,306.66999999999996)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,267.35)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,228.02)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,188.7)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,149.38)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,110.05)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path></g></g><g class="zerolinelayer"><path class="yzl zl crisp" transform="translate(0,424.64)" d="M80,0h424" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path></g><path class="xlines-below"></path><path class="ylines-below"></path><g class="overlines-below"></g><g class="xaxislayer-below"></g><g class="yaxislayer-below"></g><g class="overaxes-below"></g><g class="plot" transform="translate(80, 100)" clip-path="url('#clipfa0e2bxyplot')"><g class="scatterlayer mlayer"><g class="trace scatter trace2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2" style="stroke-miterlimit: 2; opacity: 1;"><g class="fills"></g><g class="errorbars"></g><g class="lines"></g><g class="points"><path class="point" transform="translate(36.82,228.96)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(54, 150, 161); fill-opacity: 1;"></path><path class="point" transform="translate(71.86,32.88)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(217, 30, 30); fill-opacity: 1;"></path><path class="point" transform="translate(106.89,300.03)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 66, 141); fill-opacity: 1;"></path><path class="point" transform="translate(141.93,312.12)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 51, 131); fill-opacity: 1;"></path><path class="point" transform="translate(176.96,281.18)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 89, 155); fill-opacity: 1;"></path><path class="point" transform="translate(212,302.5)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 63, 139); fill-opacity: 1;"></path><path class="point" transform="translate(247.03,288.55)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 80, 150); fill-opacity: 1;"></path><path class="point" transform="translate(282.07,308.67)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 55, 134); fill-opacity: 1;"></path><path class="point" transform="translate(317.1,188.67)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(188, 194, 86); fill-opacity: 1;"></path><path class="point" transform="translate(352.14,286.47)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 82, 151); fill-opacity: 1;"></path><path class="point" transform="translate(387.17,254.57)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(10, 121, 176); fill-opacity: 1;"></path></g><g class="text"></g></g></g></g><g class="overplot"></g><path class="xlines-above crisp" style="fill: none;" d="M0,0"></path><path class="ylines-above crisp" style="fill: none; stroke-width: 1px; stroke: rgb(0, 0, 0); stroke-opacity: 0;" d=""></path><g class="overlines-above"></g><g class="xaxislayer-above"><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Pclass" data-math="N" transform="translate(116.82,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Pclass</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Sex" data-math="N" transform="translate(151.86,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Sex</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Age" data-math="N" transform="translate(186.89,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Age</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Parch" data-math="N" transform="translate(221.93,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Parch</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Fare" data-math="N" transform="translate(256.96000000000004,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Fare</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Embarked" data-math="N" transform="translate(292,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Embarked</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Name_length" data-math="N" transform="translate(327.03,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Name_length</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Has_Cabin" data-math="N" transform="translate(362.07,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Has_Cabin</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="FamilySize" data-math="N" transform="translate(397.1,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">FamilySize</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="IsAlone" data-math="N" transform="translate(432.14,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">IsAlone</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Title" data-math="N" transform="translate(467.17,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Title</text></g></g><g class="yaxislayer-above"><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,424.64)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,385.32)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,346)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,306.66999999999996)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,267.35)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,228.02)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,188.7)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,149.38)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,110.05)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0" data-math="N" transform="translate(0,424.64)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.05" data-math="N" transform="translate(0,385.32)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.05</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.1" data-math="N" transform="translate(0,346)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.1</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.15" data-math="N" transform="translate(0,306.66999999999996)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.15</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.2" data-math="N" transform="translate(0,267.35)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.2</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.25" data-math="N" transform="translate(0,228.02)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.25</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.3" data-math="N" transform="translate(0,188.7)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.3</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.35" data-math="N" transform="translate(0,149.38)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.35</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.4" data-math="N" transform="translate(0,110.05)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.4</text></g></g><g class="overaxes-above"></g></g></g><g class="polarlayer"></g><g class="ternarylayer"></g><g class="geolayer"></g><g class="funnelarealayer"></g><g class="pielayer"></g><g class="sunburstlayer"></g><g class="glimages"></g></svg><div class="gl-container"></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><defs id="topdefs-fa0e2b"><g class="clips"></g></defs><g class="indicatorlayer"></g><g class="layer-above"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="infolayer"><g class="cb2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2 colorbar" transform="translate(80,100)"><rect class="cbbg" x="431.5" y="-0.5" width="78.640625" height="346" style="fill: rgb(0, 0, 0); fill-opacity: 0; stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 0;"></rect><g class="cbfills" transform="translate(0,10)"><rect class="cbfill" x="442" width="30" y="0" height="325" style="stroke: none; fill: url(&quot;#gfa0e2b-cb2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2&quot;);"></rect></g><g class="cblines" transform="translate(0,10)"></g><g class="cbaxis crisp" transform="translate(0,-100)"><g class="ycb2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.05" data-math="N" transform="translate(0,403.8)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.05</text></g><g class="ycb2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.1" data-math="N" transform="translate(0,358.03999999999996)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.1</text></g><g class="ycb2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.15" data-math="N" transform="translate(0,312.27)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.15</text></g><g class="ycb2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.2" data-math="N" transform="translate(0,266.5)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.2</text></g><g class="ycb2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.25" data-math="N" transform="translate(0,220.74)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.25</text></g><g class="ycb2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.3" data-math="N" transform="translate(0,174.97)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.3</text></g><g class="ycb2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.35" data-math="N" transform="translate(0,129.21)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.35</text></g></g><g class="cbtitleunshift" transform="translate(-80,-100)"><g class="cbtitle"></g></g><rect class="cboutline" x="442" y="10" width="30" height="325" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; fill: none; stroke-width: 1;"></rect></g><g class="g-gtitle"><text class="gtitle" x="301.422" y="50" text-anchor="middle" dy="0em" data-unformatted="Extra Trees Feature Importance" data-math="N" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 17px; fill: rgb(68, 68, 68); opacity: 1; font-weight: normal; white-space: pre;">Extra Trees Feature Importance</text></g><g class="g-xtitle"></g><g class="g-ytitle" transform="translate(-2.65625,0)"><text class="ytitle" transform="rotate(-90,42,272.5)" x="42" y="272.5" text-anchor="middle" data-unformatted="Feature Importance" data-math="N" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 14px; fill: rgb(68, 68, 68); opacity: 1; font-weight: normal; white-space: pre;">Feature Importance</text></g></g><g class="menulayer"></g><g class="zoomlayer"></g></svg><div class="modebar-container" style="position: absolute; top: 0px; right: 0px; width: 100%;"><div id="modebar-fa0e2b" class="modebar modebar--hover ease-bg"><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Download plot as a png" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m500 450c-83 0-150-67-150-150 0-83 67-150 150-150 83 0 150 67 150 150 0 83-67 150-150 150z m400 150h-120c-16 0-34 13-39 29l-31 93c-6 15-23 28-40 28h-340c-16 0-34-13-39-28l-31-94c-6-15-23-28-40-28h-120c-55 0-100-45-100-100v-450c0-55 45-100 100-100h800c55 0 100 45 100 100v450c0 55-45 100-100 100z m-400-550c-138 0-250 112-250 250 0 138 112 250 250 250 138 0 250-112 250-250 0-138-112-250-250-250z m365 380c-19 0-35 16-35 35 0 19 16 35 35 35 19 0 35-16 35-35 0-19-16-35-35-35z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn active" data-title="Zoom" data-attr="dragmode" data-val="zoom" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000-25l-250 251c40 63 63 138 63 218 0 224-182 406-407 406-224 0-406-182-406-406s183-406 407-406c80 0 155 22 218 62l250-250 125 125z m-812 250l0 438 437 0 0-438-437 0z m62 375l313 0 0-312-313 0 0 312z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Pan" data-attr="dragmode" data-val="pan" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000 350l-187 188 0-125-250 0 0 250 125 0-188 187-187-187 125 0 0-250-250 0 0 125-188-188 186-187 0 125 252 0 0-250-125 0 187-188 188 188-125 0 0 250 250 0 0-126 187 188z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Box Select" data-attr="dragmode" data-val="select" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m0 850l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m285 0l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m-857-286l0-143 143 0 0 143-143 0z m857 0l0-143 143 0 0 143-143 0z m-857-285l0-143 143 0 0 143-143 0z m857 0l0-143 143 0 0 143-143 0z m-857-286l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m285 0l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Lasso Select" data-attr="dragmode" data-val="lasso" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1031 1000" class="icon" height="1em" width="1em"><path d="m1018 538c-36 207-290 336-568 286-277-48-473-256-436-463 10-57 36-108 76-151-13-66 11-137 68-183 34-28 75-41 114-42l-55-70 0 0c-2-1-3-2-4-3-10-14-8-34 5-45 14-11 34-8 45 4 1 1 2 3 2 5l0 0 113 140c16 11 31 24 45 40 4 3 6 7 8 11 48-3 100 0 151 9 278 48 473 255 436 462z m-624-379c-80 14-149 48-197 96 42 42 109 47 156 9 33-26 47-66 41-105z m-187-74c-19 16-33 37-39 60 50-32 109-55 174-68-42-25-95-24-135 8z m360 75c-34-7-69-9-102-8 8 62-16 128-68 170-73 59-175 54-244-5-9 20-16 40-20 61-28 159 121 317 333 354s407-60 434-217c28-159-121-318-333-355z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Zoom in" data-attr="zoom" data-val="in" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m1 787l0-875 875 0 0 875-875 0z m687-500l-187 0 0-187-125 0 0 187-188 0 0 125 188 0 0 187 125 0 0-187 187 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Zoom out" data-attr="zoom" data-val="out" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m0 788l0-876 875 0 0 876-875 0z m688-500l-500 0 0 125 500 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Autoscale" data-attr="zoom" data-val="auto" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m250 850l-187 0-63 0 0-62 0-188 63 0 0 188 187 0 0 62z m688 0l-188 0 0-62 188 0 0-188 62 0 0 188 0 62-62 0z m-875-938l0 188-63 0 0-188 0-62 63 0 187 0 0 62-187 0z m875 188l0-188-188 0 0-62 188 0 62 0 0 62 0 188-62 0z m-125 188l-1 0-93-94-156 156 156 156 92-93 2 0 0 250-250 0 0-2 93-92-156-156-156 156 94 92 0 2-250 0 0-250 0 0 93 93 157-156-157-156-93 94 0 0 0-250 250 0 0 0-94 93 156 157 156-157-93-93 0 0 250 0 0 250z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Reset axes" data-attr="zoom" data-val="reset" data-toggle="false" data-gravity="n"><svg viewBox="0 0 928.6 1000" class="icon" height="1em" width="1em"><path d="m786 296v-267q0-15-11-26t-25-10h-214v214h-143v-214h-214q-15 0-25 10t-11 26v267q0 1 0 2t0 2l321 264 321-264q1-1 1-4z m124 39l-34-41q-5-5-12-6h-2q-7 0-12 3l-386 322-386-322q-7-4-13-4-7 2-12 7l-35 41q-4 5-3 13t6 12l401 334q18 15 42 15t43-15l136-114v109q0 8 5 13t13 5h107q8 0 13-5t5-13v-227l122-102q5-5 6-12t-4-13z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Toggle Spike Lines" data-attr="_cartesianSpikesEnabled" data-val="on" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="M512 409c0-57-46-104-103-104-57 0-104 47-104 104 0 57 47 103 104 103 57 0 103-46 103-103z m-327-39l92 0 0 92-92 0z m-185 0l92 0 0 92-92 0z m370-186l92 0 0 93-92 0z m0-184l92 0 0 92-92 0z" transform="matrix(1.5 0 0 -1.5 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn active" data-title="Show closest data on hover" data-attr="hovermode" data-val="closest" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1500 1000" class="icon" height="1em" width="1em"><path d="m375 725l0 0-375-375 375-374 0-1 1125 0 0 750-1125 0z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Compare data on hover" data-attr="hovermode" data-val="x" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1125 1000" class="icon" height="1em" width="1em"><path d="m187 786l0 2-187-188 188-187 0 0 937 0 0 373-938 0z m0-499l0 1-187-188 188-188 0 0 937 0 0 376-938-1z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a href="https://plot.ly/" target="_blank" data-title="Produced with Plotly" class="modebar-btn plotlyjsicon modebar-btn--logo"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 132 132" height="1em" width="1em"><defs><style>.cls-1 {fill: #119dff;} .cls-2 {fill: #25fefd;} .cls-3 {fill: #fff;}</style></defs><title>plotly-logomark</title><g id="symbol"><rect class="cls-1" width="132" height="132" rx="6" ry="6"></rect><circle class="cls-2" cx="78" cy="54" r="6"></circle><circle class="cls-2" cx="102" cy="30" r="6"></circle><circle class="cls-2" cx="78" cy="30" r="6"></circle><circle class="cls-2" cx="54" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="54" r="6"></circle><path class="cls-3" d="M30,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,30,72Z"></path><path class="cls-3" d="M78,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,78,72Z"></path><path class="cls-3" d="M54,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,54,48Z"></path><path class="cls-3" d="M102,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,102,48Z"></path></g></svg></a></div></div></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><g class="hoverlayer"></g></svg></div></div></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';
                    
                if (document.getElementById("8f074751-16da-4a03-aab5-567c726922d0")) {
                    Plotly.newPlot(
                        '8f074751-16da-4a03-aab5-567c726922d0',
                        [{"marker": {"color": [0.12165657, 0.37098307, 0.03129623, 0.01591611, 0.05525811, 0.028157, 0.04589793, 0.02030357, 0.17289562, 0.04853517, 0.08910063], "colorscale": "Portland", "showscale": true, "size": 25, "sizemode": "diameter", "sizeref": 1}, "mode": "markers", "text": ["Pclass", "Sex", "Age", "Parch", "Fare", "Embarked", "Name_length", "Has_Cabin", "FamilySize", "IsAlone", "Title"], "type": "scatter", "uid": "2c4a0ba2-93f0-4967-a8d6-13a1cdbc52b2", "x": ["Pclass", "Sex", "Age", "Parch", "Fare", "Embarked", "Name_length", "Has_Cabin", "FamilySize", "IsAlone", "Title"], "y": [0.12165657, 0.37098307, 0.03129623, 0.01591611, 0.05525811, 0.028157, 0.04589793, 0.02030357, 0.17289562, 0.04853517, 0.08910063]}],
                        {"autosize": true, "hovermode": "closest", "showlegend": false, "title": {"text": "Extra Trees Feature Importance"}, "yaxis": {"gridwidth": 2, "ticklen": 5, "title": {"text": "Feature Importance"}}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){
                            
var gd = document.getElementById('8f074751-16da-4a03-aab5-567c726922d0');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>



<div class="output_html rendered_html output_subarea ">
<div>
        
        
            <div id="421bfae6-c82f-4c1b-8f37-aea308c7d70b" class="plotly-graph-div js-plotly-plot" style="height:525px; width:100%;"><div class="plot-container plotly"><div class="svg-container" style="position: relative; width: 100%; height: 100%;"><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525" style="background: rgb(255, 255, 255);"><defs id="defs-971995"><g class="clips"><clipPath id="clip971995xyplot" class="plotclip"><rect width="431" height="345"></rect></clipPath><clipPath class="axesclip" id="clip971995x"><rect x="80" y="0" width="431" height="525"></rect></clipPath><clipPath class="axesclip" id="clip971995y"><rect x="0" y="100" width="602.844" height="345"></rect></clipPath><clipPath class="axesclip" id="clip971995xy"><rect x="80" y="100" width="431" height="345"></rect></clipPath></g><g class="gradients"><linearGradient x1="0" x2="0" y1="1" y2="0" id="g971995-cb955ed24b-c7cd-4aa4-9a13-bd18ff95ee6b"><stop offset="0%" stop-color="rgb(12, 51, 131)" stop-opacity="1"></stop><stop offset="25%" stop-color="rgb(10, 136, 186)" stop-opacity="1"></stop><stop offset="50%" stop-color="rgb(242, 211, 56)" stop-opacity="1"></stop><stop offset="75%" stop-color="rgb(242, 143, 56)" stop-opacity="1"></stop><stop offset="100%" stop-color="rgb(217, 30, 30)" stop-opacity="1"></stop></linearGradient></g></defs><g class="bglayer"></g><g class="draglayer cursor-crosshair"><g class="xy"><rect class="nsewdrag drag" data-subplot="xy" x="80" y="100" width="431" height="345" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nwdrag drag cursor-nw-resize" data-subplot="xy" x="60" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nedrag drag cursor-ne-resize" data-subplot="xy" x="511" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="swdrag drag cursor-sw-resize" data-subplot="xy" x="60" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sedrag drag cursor-se-resize" data-subplot="xy" x="511" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ewdrag drag cursor-ew-resize" data-subplot="xy" x="123.1" y="445.5" width="344.8" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="wdrag drag cursor-w-resize" data-subplot="xy" x="80" y="445.5" width="43.1" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="edrag drag cursor-e-resize" data-subplot="xy" x="467.90000000000003" y="445.5" width="43.1" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nsdrag drag cursor-ns-resize" data-subplot="xy" x="59.5" y="134.5" width="20" height="276" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sdrag drag cursor-s-resize" data-subplot="xy" x="59.5" y="410.5" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ndrag drag cursor-n-resize" data-subplot="xy" x="59.5" y="100" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect></g></g><g class="layer-below"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="cartesianlayer"><g class="subplot xy"><g class="layer-subplot"><g class="shapelayer"></g><g class="imagelayer"></g></g><g class="gridlayer"><g class="x"><path class="xgrid crisp" transform="translate(117.18,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(152.84,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(188.51,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(224.17,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(259.84000000000003,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(295.5,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(331.16999999999996,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(366.83,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(402.5,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(438.16,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(473.83,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path></g><g class="y"><path class="ygrid crisp" transform="translate(0,374.32)" d="M80,0h431" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,336.52)" d="M80,0h431" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,298.71000000000004)" d="M80,0h431" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,260.90999999999997)" d="M80,0h431" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,223.1)" d="M80,0h431" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,185.3)" d="M80,0h431" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,147.49)" d="M80,0h431" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,109.69)" d="M80,0h431" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path></g></g><g class="zerolinelayer"><path class="yzl zl crisp" transform="translate(0,412.13)" d="M80,0h431" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path></g><path class="xlines-below"></path><path class="ylines-below"></path><g class="overlines-below"></g><g class="xaxislayer-below"></g><g class="yaxislayer-below"></g><g class="overaxes-below"></g><g class="plot" transform="translate(80, 100)" clip-path="url('#clip971995xyplot')"><g class="scatterlayer mlayer"><g class="trace scatter trace955ed24b-c7cd-4aa4-9a13-bd18ff95ee6b" style="stroke-miterlimit: 2; opacity: 1;"><g class="fills"></g><g class="errorbars"></g><g class="lines"></g><g class="points"><path class="point" transform="translate(37.18,301.54)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 64, 139); fill-opacity: 1;"></path><path class="point" transform="translate(72.84,309.1)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 55, 133); fill-opacity: 1;"></path><path class="point" transform="translate(108.51,307.59)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 57, 135); fill-opacity: 1;"></path><path class="point" transform="translate(144.17,289.95)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 78, 148); fill-opacity: 1;"></path><path class="point" transform="translate(179.84,300.03)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 66, 141); fill-opacity: 1;"></path><path class="point" transform="translate(215.5,309.1)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 55, 133); fill-opacity: 1;"></path><path class="point" transform="translate(251.17,294.48)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 72, 145); fill-opacity: 1;"></path><path class="point" transform="translate(286.83,312.13)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 51, 131); fill-opacity: 1;"></path><path class="point" transform="translate(322.5,290.45)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 77, 148); fill-opacity: 1;"></path><path class="point" transform="translate(358.16,32.88)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(217, 30, 30); fill-opacity: 1;"></path><path class="point" transform="translate(393.83,308.09)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 56, 134); fill-opacity: 1;"></path></g><g class="text"></g></g></g></g><g class="overplot"></g><path class="xlines-above crisp" style="fill: none;" d="M0,0"></path><path class="ylines-above crisp" style="fill: none; stroke-width: 1px; stroke: rgb(0, 0, 0); stroke-opacity: 0;" d=""></path><g class="overlines-above"></g><g class="xaxislayer-above"><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Pclass" data-math="N" transform="translate(117.18,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Pclass</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Sex" data-math="N" transform="translate(152.84,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Sex</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Age" data-math="N" transform="translate(188.51,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Age</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Parch" data-math="N" transform="translate(224.17,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Parch</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Fare" data-math="N" transform="translate(259.84000000000003,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Fare</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Embarked" data-math="N" transform="translate(295.5,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Embarked</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Name_length" data-math="N" transform="translate(331.16999999999996,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Name_length</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Has_Cabin" data-math="N" transform="translate(366.83,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Has_Cabin</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="FamilySize" data-math="N" transform="translate(402.5,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">FamilySize</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="IsAlone" data-math="N" transform="translate(438.16,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">IsAlone</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Title" data-math="N" transform="translate(473.83,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Title</text></g></g><g class="yaxislayer-above"><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,412.13)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,374.32)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,336.52)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,298.71000000000004)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,260.90999999999997)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,223.1)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,185.3)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,147.49)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,109.69)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0" data-math="N" transform="translate(0,412.13)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.1" data-math="N" transform="translate(0,374.32)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.1</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.2" data-math="N" transform="translate(0,336.52)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.2</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.3" data-math="N" transform="translate(0,298.71000000000004)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.3</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.4" data-math="N" transform="translate(0,260.90999999999997)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.4</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.5" data-math="N" transform="translate(0,223.1)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.5</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.6" data-math="N" transform="translate(0,185.3)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.6</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.7" data-math="N" transform="translate(0,147.49)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.7</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.8" data-math="N" transform="translate(0,109.69)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.8</text></g></g><g class="overaxes-above"></g></g></g><g class="polarlayer"></g><g class="ternarylayer"></g><g class="geolayer"></g><g class="funnelarealayer"></g><g class="pielayer"></g><g class="sunburstlayer"></g><g class="glimages"></g></svg><div class="gl-container"></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><defs id="topdefs-971995"><g class="clips"></g></defs><g class="indicatorlayer"></g><g class="layer-above"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="infolayer"><g class="cb955ed24b-c7cd-4aa4-9a13-bd18ff95ee6b colorbar" transform="translate(80,100)"><rect class="cbbg" x="439.5" y="-0.5" width="71.015625" height="346" style="fill: rgb(0, 0, 0); fill-opacity: 0; stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 0;"></rect><g class="cbfills" transform="translate(0,10)"><rect class="cbfill" x="450" width="30" y="0" height="325" style="stroke: none; fill: url(&quot;#g971995-cb955ed24b-c7cd-4aa4-9a13-bd18ff95ee6b&quot;);"></rect></g><g class="cblines" transform="translate(0,10)"></g><g class="cbaxis crisp" transform="translate(0,-100)"><g class="ycb955ed24b-c7cd-4aa4-9a13-bd18ff95ee6btick"><text text-anchor="start" x="483.4" y="4.199999999999999" data-unformatted="0" data-math="N" transform="translate(0,435)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0</text></g><g class="ycb955ed24b-c7cd-4aa4-9a13-bd18ff95ee6btick"><text text-anchor="start" x="483.4" y="4.199999999999999" data-unformatted="0.1" data-math="N" transform="translate(0,391)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.1</text></g><g class="ycb955ed24b-c7cd-4aa4-9a13-bd18ff95ee6btick"><text text-anchor="start" x="483.4" y="4.199999999999999" data-unformatted="0.2" data-math="N" transform="translate(0,347)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.2</text></g><g class="ycb955ed24b-c7cd-4aa4-9a13-bd18ff95ee6btick"><text text-anchor="start" x="483.4" y="4.199999999999999" data-unformatted="0.3" data-math="N" transform="translate(0,303.01)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.3</text></g><g class="ycb955ed24b-c7cd-4aa4-9a13-bd18ff95ee6btick"><text text-anchor="start" x="483.4" y="4.199999999999999" data-unformatted="0.4" data-math="N" transform="translate(0,259.01)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.4</text></g><g class="ycb955ed24b-c7cd-4aa4-9a13-bd18ff95ee6btick"><text text-anchor="start" x="483.4" y="4.199999999999999" data-unformatted="0.5" data-math="N" transform="translate(0,215.01)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.5</text></g><g class="ycb955ed24b-c7cd-4aa4-9a13-bd18ff95ee6btick"><text text-anchor="start" x="483.4" y="4.199999999999999" data-unformatted="0.6" data-math="N" transform="translate(0,171.01)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.6</text></g><g class="ycb955ed24b-c7cd-4aa4-9a13-bd18ff95ee6btick"><text text-anchor="start" x="483.4" y="4.199999999999999" data-unformatted="0.7" data-math="N" transform="translate(0,127.01)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.7</text></g></g><g class="cbtitleunshift" transform="translate(-80,-100)"><g class="cbtitle"></g></g><rect class="cboutline" x="450" y="10" width="30" height="325" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; fill: none; stroke-width: 1;"></rect></g><g class="g-gtitle"><text class="gtitle" x="301.422" y="50" text-anchor="middle" dy="0em" data-unformatted="AdaBoost Feature Importance" data-math="N" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 17px; fill: rgb(68, 68, 68); opacity: 1; font-weight: normal; white-space: pre;">AdaBoost Feature Importance</text></g><g class="g-xtitle"></g><g class="g-ytitle"><text class="ytitle" transform="rotate(-90,42,272.5)" x="42" y="272.5" text-anchor="middle" data-unformatted="Feature Importance" data-math="N" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 14px; fill: rgb(68, 68, 68); opacity: 1; font-weight: normal; white-space: pre;">Feature Importance</text></g></g><g class="menulayer"></g><g class="zoomlayer"></g></svg><div class="modebar-container" style="position: absolute; top: 0px; right: 0px; width: 100%;"><div id="modebar-971995" class="modebar modebar--hover ease-bg"><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Download plot as a png" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m500 450c-83 0-150-67-150-150 0-83 67-150 150-150 83 0 150 67 150 150 0 83-67 150-150 150z m400 150h-120c-16 0-34 13-39 29l-31 93c-6 15-23 28-40 28h-340c-16 0-34-13-39-28l-31-94c-6-15-23-28-40-28h-120c-55 0-100-45-100-100v-450c0-55 45-100 100-100h800c55 0 100 45 100 100v450c0 55-45 100-100 100z m-400-550c-138 0-250 112-250 250 0 138 112 250 250 250 138 0 250-112 250-250 0-138-112-250-250-250z m365 380c-19 0-35 16-35 35 0 19 16 35 35 35 19 0 35-16 35-35 0-19-16-35-35-35z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn active" data-title="Zoom" data-attr="dragmode" data-val="zoom" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000-25l-250 251c40 63 63 138 63 218 0 224-182 406-407 406-224 0-406-182-406-406s183-406 407-406c80 0 155 22 218 62l250-250 125 125z m-812 250l0 438 437 0 0-438-437 0z m62 375l313 0 0-312-313 0 0 312z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Pan" data-attr="dragmode" data-val="pan" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000 350l-187 188 0-125-250 0 0 250 125 0-188 187-187-187 125 0 0-250-250 0 0 125-188-188 186-187 0 125 252 0 0-250-125 0 187-188 188 188-125 0 0 250 250 0 0-126 187 188z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Box Select" data-attr="dragmode" data-val="select" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m0 850l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m285 0l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m-857-286l0-143 143 0 0 143-143 0z m857 0l0-143 143 0 0 143-143 0z m-857-285l0-143 143 0 0 143-143 0z m857 0l0-143 143 0 0 143-143 0z m-857-286l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m285 0l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Lasso Select" data-attr="dragmode" data-val="lasso" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1031 1000" class="icon" height="1em" width="1em"><path d="m1018 538c-36 207-290 336-568 286-277-48-473-256-436-463 10-57 36-108 76-151-13-66 11-137 68-183 34-28 75-41 114-42l-55-70 0 0c-2-1-3-2-4-3-10-14-8-34 5-45 14-11 34-8 45 4 1 1 2 3 2 5l0 0 113 140c16 11 31 24 45 40 4 3 6 7 8 11 48-3 100 0 151 9 278 48 473 255 436 462z m-624-379c-80 14-149 48-197 96 42 42 109 47 156 9 33-26 47-66 41-105z m-187-74c-19 16-33 37-39 60 50-32 109-55 174-68-42-25-95-24-135 8z m360 75c-34-7-69-9-102-8 8 62-16 128-68 170-73 59-175 54-244-5-9 20-16 40-20 61-28 159 121 317 333 354s407-60 434-217c28-159-121-318-333-355z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Zoom in" data-attr="zoom" data-val="in" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m1 787l0-875 875 0 0 875-875 0z m687-500l-187 0 0-187-125 0 0 187-188 0 0 125 188 0 0 187 125 0 0-187 187 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Zoom out" data-attr="zoom" data-val="out" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m0 788l0-876 875 0 0 876-875 0z m688-500l-500 0 0 125 500 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Autoscale" data-attr="zoom" data-val="auto" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m250 850l-187 0-63 0 0-62 0-188 63 0 0 188 187 0 0 62z m688 0l-188 0 0-62 188 0 0-188 62 0 0 188 0 62-62 0z m-875-938l0 188-63 0 0-188 0-62 63 0 187 0 0 62-187 0z m875 188l0-188-188 0 0-62 188 0 62 0 0 62 0 188-62 0z m-125 188l-1 0-93-94-156 156 156 156 92-93 2 0 0 250-250 0 0-2 93-92-156-156-156 156 94 92 0 2-250 0 0-250 0 0 93 93 157-156-157-156-93 94 0 0 0-250 250 0 0 0-94 93 156 157 156-157-93-93 0 0 250 0 0 250z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Reset axes" data-attr="zoom" data-val="reset" data-toggle="false" data-gravity="n"><svg viewBox="0 0 928.6 1000" class="icon" height="1em" width="1em"><path d="m786 296v-267q0-15-11-26t-25-10h-214v214h-143v-214h-214q-15 0-25 10t-11 26v267q0 1 0 2t0 2l321 264 321-264q1-1 1-4z m124 39l-34-41q-5-5-12-6h-2q-7 0-12 3l-386 322-386-322q-7-4-13-4-7 2-12 7l-35 41q-4 5-3 13t6 12l401 334q18 15 42 15t43-15l136-114v109q0 8 5 13t13 5h107q8 0 13-5t5-13v-227l122-102q5-5 6-12t-4-13z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Toggle Spike Lines" data-attr="_cartesianSpikesEnabled" data-val="on" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="M512 409c0-57-46-104-103-104-57 0-104 47-104 104 0 57 47 103 104 103 57 0 103-46 103-103z m-327-39l92 0 0 92-92 0z m-185 0l92 0 0 92-92 0z m370-186l92 0 0 93-92 0z m0-184l92 0 0 92-92 0z" transform="matrix(1.5 0 0 -1.5 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn active" data-title="Show closest data on hover" data-attr="hovermode" data-val="closest" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1500 1000" class="icon" height="1em" width="1em"><path d="m375 725l0 0-375-375 375-374 0-1 1125 0 0 750-1125 0z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Compare data on hover" data-attr="hovermode" data-val="x" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1125 1000" class="icon" height="1em" width="1em"><path d="m187 786l0 2-187-188 188-187 0 0 937 0 0 373-938 0z m0-499l0 1-187-188 188-188 0 0 937 0 0 376-938-1z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a href="https://plot.ly/" target="_blank" data-title="Produced with Plotly" class="modebar-btn plotlyjsicon modebar-btn--logo"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 132 132" height="1em" width="1em"><defs><style>.cls-1 {fill: #119dff;} .cls-2 {fill: #25fefd;} .cls-3 {fill: #fff;}</style></defs><title>plotly-logomark</title><g id="symbol"><rect class="cls-1" width="132" height="132" rx="6" ry="6"></rect><circle class="cls-2" cx="78" cy="54" r="6"></circle><circle class="cls-2" cx="102" cy="30" r="6"></circle><circle class="cls-2" cx="78" cy="30" r="6"></circle><circle class="cls-2" cx="54" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="54" r="6"></circle><path class="cls-3" d="M30,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,30,72Z"></path><path class="cls-3" d="M78,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,78,72Z"></path><path class="cls-3" d="M54,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,54,48Z"></path><path class="cls-3" d="M102,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,102,48Z"></path></g></svg></a></div></div></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><g class="hoverlayer"></g></svg></div></div></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';
                    
                if (document.getElementById("421bfae6-c82f-4c1b-8f37-aea308c7d70b")) {
                    Plotly.newPlot(
                        '421bfae6-c82f-4c1b-8f37-aea308c7d70b',
                        [{"marker": {"color": [0.028, 0.008, 0.012, 0.05866667, 0.032, 0.008, 0.04666667, 0.0, 0.05733333, 0.73866667, 0.01066667], "colorscale": "Portland", "showscale": true, "size": 25, "sizemode": "diameter", "sizeref": 1}, "mode": "markers", "text": ["Pclass", "Sex", "Age", "Parch", "Fare", "Embarked", "Name_length", "Has_Cabin", "FamilySize", "IsAlone", "Title"], "type": "scatter", "uid": "955ed24b-c7cd-4aa4-9a13-bd18ff95ee6b", "x": ["Pclass", "Sex", "Age", "Parch", "Fare", "Embarked", "Name_length", "Has_Cabin", "FamilySize", "IsAlone", "Title"], "y": [0.028, 0.008, 0.012, 0.05866667, 0.032, 0.008, 0.04666667, 0.0, 0.05733333, 0.73866667, 0.01066667]}],
                        {"autosize": true, "hovermode": "closest", "showlegend": false, "title": {"text": "AdaBoost Feature Importance"}, "yaxis": {"gridwidth": 2, "ticklen": 5, "title": {"text": "Feature Importance"}}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){
                            
var gd = document.getElementById('421bfae6-c82f-4c1b-8f37-aea308c7d70b');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>
</div>

</div>

<div class="output_area">

    <div class="prompt"></div>



<div class="output_html rendered_html output_subarea ">
<div>
        
        
            <div id="ed256dea-4bf4-4336-a21a-74d5a5fb4518" class="plotly-graph-div js-plotly-plot" style="height:525px; width:100%;"><div class="plot-container plotly"><div class="svg-container" style="position: relative; width: 100%; height: 100%;"><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525" style="background: rgb(255, 255, 255);"><defs id="defs-02f7ae"><g class="clips"><clipPath id="clip02f7aexyplot" class="plotclip"><rect width="424" height="345"></rect></clipPath><clipPath class="axesclip" id="clip02f7aex"><rect x="80" y="0" width="424" height="525"></rect></clipPath><clipPath class="axesclip" id="clip02f7aey"><rect x="0" y="100" width="602.844" height="345"></rect></clipPath><clipPath class="axesclip" id="clip02f7aexy"><rect x="80" y="100" width="424" height="345"></rect></clipPath></g><g class="gradients"><linearGradient x1="0" x2="0" y1="1" y2="0" id="g02f7ae-cb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6"><stop offset="0%" stop-color="rgb(12, 51, 131)" stop-opacity="1"></stop><stop offset="25%" stop-color="rgb(10, 136, 186)" stop-opacity="1"></stop><stop offset="50%" stop-color="rgb(242, 211, 56)" stop-opacity="1"></stop><stop offset="75%" stop-color="rgb(242, 143, 56)" stop-opacity="1"></stop><stop offset="100%" stop-color="rgb(217, 30, 30)" stop-opacity="1"></stop></linearGradient></g></defs><g class="bglayer"></g><g class="draglayer cursor-crosshair"><g class="xy"><rect class="nsewdrag drag" data-subplot="xy" x="80" y="100" width="424" height="345" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nwdrag drag cursor-nw-resize" data-subplot="xy" x="60" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nedrag drag cursor-ne-resize" data-subplot="xy" x="504" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="swdrag drag cursor-sw-resize" data-subplot="xy" x="60" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sedrag drag cursor-se-resize" data-subplot="xy" x="504" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ewdrag drag cursor-ew-resize" data-subplot="xy" x="122.4" y="445.5" width="339.20000000000005" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="wdrag drag cursor-w-resize" data-subplot="xy" x="80" y="445.5" width="42.400000000000006" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="edrag drag cursor-e-resize" data-subplot="xy" x="461.6" y="445.5" width="42.400000000000006" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nsdrag drag cursor-ns-resize" data-subplot="xy" x="59.5" y="134.5" width="20" height="276" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sdrag drag cursor-s-resize" data-subplot="xy" x="59.5" y="410.5" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ndrag drag cursor-n-resize" data-subplot="xy" x="59.5" y="100" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect></g></g><g class="layer-below"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="cartesianlayer"><g class="subplot xy"><g class="layer-subplot"><g class="shapelayer"></g><g class="imagelayer"></g></g><g class="gridlayer"><g class="x"><path class="xgrid crisp" transform="translate(116.82,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(151.86,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(186.89,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(221.93,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(256.96000000000004,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(292,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(327.03,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(362.07,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(397.1,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(432.14,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(467.17,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path></g><g class="y"><path class="ygrid crisp" transform="translate(0,357.89)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,295.27)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,232.64)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,170.01999999999998)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,107.4)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path></g></g><g class="zerolinelayer"><path class="yzl zl crisp" transform="translate(0,420.52)" d="M80,0h424" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path></g><path class="xlines-below"></path><path class="ylines-below"></path><g class="overlines-below"></g><g class="xaxislayer-below"></g><g class="yaxislayer-below"></g><g class="overaxes-below"></g><g class="plot" transform="translate(80, 100)" clip-path="url('#clip02f7aexyplot')"><g class="scatterlayer mlayer"><g class="trace scatter trace6ab66e1c-b36b-48b5-80ba-aafff65fbdc6" style="stroke-miterlimit: 2; opacity: 1;"><g class="fills"></g><g class="errorbars"></g><g class="lines"></g><g class="points"><path class="point" transform="translate(36.82,277.96)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 93, 158); fill-opacity: 1;"></path><path class="point" transform="translate(71.86,296.16)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 70, 144); fill-opacity: 1;"></path><path class="point" transform="translate(106.89,275.19)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 96, 160); fill-opacity: 1;"></path><path class="point" transform="translate(141.93,304.06)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 61, 137); fill-opacity: 1;"></path><path class="point" transform="translate(176.96,250.41)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(10, 126, 180); fill-opacity: 1;"></path><path class="point" transform="translate(212,290.59)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 77, 148); fill-opacity: 1;"></path><path class="point" transform="translate(247.03,283.16)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 86, 154); fill-opacity: 1;"></path><path class="point" transform="translate(282.07,303.14)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 62, 138); fill-opacity: 1;"></path><path class="point" transform="translate(317.1,273.78)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(11, 98, 161); fill-opacity: 1;"></path><path class="point" transform="translate(352.14,32.88)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(217, 30, 30); fill-opacity: 1;"></path><path class="point" transform="translate(387.17,312.13)" d="M12.5,0A12.5,12.5 0 1,1 0,-12.5A12.5,12.5 0 0,1 12.5,0Z" style="opacity: 1; stroke-width: 0px; fill: rgb(12, 51, 131); fill-opacity: 1;"></path></g><g class="text"></g></g></g></g><g class="overplot"></g><path class="xlines-above crisp" style="fill: none;" d="M0,0"></path><path class="ylines-above crisp" style="fill: none; stroke-width: 1px; stroke: rgb(0, 0, 0); stroke-opacity: 0;" d=""></path><g class="overlines-above"></g><g class="xaxislayer-above"><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Pclass" data-math="N" transform="translate(116.82,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Pclass</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Sex" data-math="N" transform="translate(151.86,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Sex</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Age" data-math="N" transform="translate(186.89,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Age</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Parch" data-math="N" transform="translate(221.93,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Parch</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Fare" data-math="N" transform="translate(256.96000000000004,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Fare</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Embarked" data-math="N" transform="translate(292,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Embarked</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Name_length" data-math="N" transform="translate(327.03,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Name_length</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Has_Cabin" data-math="N" transform="translate(362.07,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Has_Cabin</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="FamilySize" data-math="N" transform="translate(397.1,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">FamilySize</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="IsAlone" data-math="N" transform="translate(432.14,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">IsAlone</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Title" data-math="N" transform="translate(467.17,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Title</text></g></g><g class="yaxislayer-above"><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,420.52)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,357.89)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,295.27)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,232.64)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,170.01999999999998)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,107.4)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0" data-math="N" transform="translate(0,420.52)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.1" data-math="N" transform="translate(0,357.89)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.1</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.2" data-math="N" transform="translate(0,295.27)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.2</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.3" data-math="N" transform="translate(0,232.64)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.3</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.4" data-math="N" transform="translate(0,170.01999999999998)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.4</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.5" data-math="N" transform="translate(0,107.4)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.5</text></g></g><g class="overaxes-above"></g></g></g><g class="polarlayer"></g><g class="ternarylayer"></g><g class="geolayer"></g><g class="funnelarealayer"></g><g class="pielayer"></g><g class="sunburstlayer"></g><g class="glimages"></g></svg><div class="gl-container"></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><defs id="topdefs-02f7ae"><g class="clips"></g></defs><g class="indicatorlayer"></g><g class="layer-above"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="infolayer"><g class="cb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6 colorbar" transform="translate(80,100)"><rect class="cbbg" x="431.5" y="-0.5" width="78.640625" height="346" style="fill: rgb(0, 0, 0); fill-opacity: 0; stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 0;"></rect><g class="cbfills" transform="translate(0,10)"><rect class="cbfill" x="442" width="30" y="0" height="325" style="stroke: none; fill: url(&quot;#g02f7ae-cb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6&quot;);"></rect></g><g class="cblines" transform="translate(0,10)"></g><g class="cbaxis crisp" transform="translate(0,-100)"><g class="ycb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.05" data-math="N" transform="translate(0,408.33)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.05</text></g><g class="ycb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.1" data-math="N" transform="translate(0,371.88)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.1</text></g><g class="ycb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.15" data-math="N" transform="translate(0,335.44)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.15</text></g><g class="ycb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.2" data-math="N" transform="translate(0,299)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.2</text></g><g class="ycb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.25" data-math="N" transform="translate(0,262.56)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.25</text></g><g class="ycb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.3" data-math="N" transform="translate(0,226.11)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.3</text></g><g class="ycb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.35" data-math="N" transform="translate(0,189.67000000000002)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.35</text></g><g class="ycb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.4" data-math="N" transform="translate(0,153.23)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.4</text></g><g class="ycb6ab66e1c-b36b-48b5-80ba-aafff65fbdc6tick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.45" data-math="N" transform="translate(0,116.79)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.45</text></g></g><g class="cbtitleunshift" transform="translate(-80,-100)"><g class="cbtitle"></g></g><rect class="cboutline" x="442" y="10" width="30" height="325" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; fill: none; stroke-width: 1;"></rect></g><g class="g-gtitle"><text class="gtitle" x="301.422" y="50" text-anchor="middle" dy="0em" data-unformatted="Gradient Boosting Feature Importance" data-math="N" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 17px; fill: rgb(68, 68, 68); opacity: 1; font-weight: normal; white-space: pre;">Gradient Boosting Feature Importance</text></g><g class="g-xtitle"></g><g class="g-ytitle"><text class="ytitle" transform="rotate(-90,42,272.5)" x="42" y="272.5" text-anchor="middle" data-unformatted="Feature Importance" data-math="N" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 14px; fill: rgb(68, 68, 68); opacity: 1; font-weight: normal; white-space: pre;">Feature Importance</text></g></g><g class="menulayer"></g><g class="zoomlayer"></g></svg><div class="modebar-container" style="position: absolute; top: 0px; right: 0px; width: 100%;"><div id="modebar-02f7ae" class="modebar modebar--hover ease-bg"><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Download plot as a png" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m500 450c-83 0-150-67-150-150 0-83 67-150 150-150 83 0 150 67 150 150 0 83-67 150-150 150z m400 150h-120c-16 0-34 13-39 29l-31 93c-6 15-23 28-40 28h-340c-16 0-34-13-39-28l-31-94c-6-15-23-28-40-28h-120c-55 0-100-45-100-100v-450c0-55 45-100 100-100h800c55 0 100 45 100 100v450c0 55-45 100-100 100z m-400-550c-138 0-250 112-250 250 0 138 112 250 250 250 138 0 250-112 250-250 0-138-112-250-250-250z m365 380c-19 0-35 16-35 35 0 19 16 35 35 35 19 0 35-16 35-35 0-19-16-35-35-35z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn active" data-title="Zoom" data-attr="dragmode" data-val="zoom" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000-25l-250 251c40 63 63 138 63 218 0 224-182 406-407 406-224 0-406-182-406-406s183-406 407-406c80 0 155 22 218 62l250-250 125 125z m-812 250l0 438 437 0 0-438-437 0z m62 375l313 0 0-312-313 0 0 312z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Pan" data-attr="dragmode" data-val="pan" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000 350l-187 188 0-125-250 0 0 250 125 0-188 187-187-187 125 0 0-250-250 0 0 125-188-188 186-187 0 125 252 0 0-250-125 0 187-188 188 188-125 0 0 250 250 0 0-126 187 188z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Box Select" data-attr="dragmode" data-val="select" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m0 850l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m285 0l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m-857-286l0-143 143 0 0 143-143 0z m857 0l0-143 143 0 0 143-143 0z m-857-285l0-143 143 0 0 143-143 0z m857 0l0-143 143 0 0 143-143 0z m-857-286l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m285 0l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Lasso Select" data-attr="dragmode" data-val="lasso" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1031 1000" class="icon" height="1em" width="1em"><path d="m1018 538c-36 207-290 336-568 286-277-48-473-256-436-463 10-57 36-108 76-151-13-66 11-137 68-183 34-28 75-41 114-42l-55-70 0 0c-2-1-3-2-4-3-10-14-8-34 5-45 14-11 34-8 45 4 1 1 2 3 2 5l0 0 113 140c16 11 31 24 45 40 4 3 6 7 8 11 48-3 100 0 151 9 278 48 473 255 436 462z m-624-379c-80 14-149 48-197 96 42 42 109 47 156 9 33-26 47-66 41-105z m-187-74c-19 16-33 37-39 60 50-32 109-55 174-68-42-25-95-24-135 8z m360 75c-34-7-69-9-102-8 8 62-16 128-68 170-73 59-175 54-244-5-9 20-16 40-20 61-28 159 121 317 333 354s407-60 434-217c28-159-121-318-333-355z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Zoom in" data-attr="zoom" data-val="in" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m1 787l0-875 875 0 0 875-875 0z m687-500l-187 0 0-187-125 0 0 187-188 0 0 125 188 0 0 187 125 0 0-187 187 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Zoom out" data-attr="zoom" data-val="out" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m0 788l0-876 875 0 0 876-875 0z m688-500l-500 0 0 125 500 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Autoscale" data-attr="zoom" data-val="auto" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m250 850l-187 0-63 0 0-62 0-188 63 0 0 188 187 0 0 62z m688 0l-188 0 0-62 188 0 0-188 62 0 0 188 0 62-62 0z m-875-938l0 188-63 0 0-188 0-62 63 0 187 0 0 62-187 0z m875 188l0-188-188 0 0-62 188 0 62 0 0 62 0 188-62 0z m-125 188l-1 0-93-94-156 156 156 156 92-93 2 0 0 250-250 0 0-2 93-92-156-156-156 156 94 92 0 2-250 0 0-250 0 0 93 93 157-156-157-156-93 94 0 0 0-250 250 0 0 0-94 93 156 157 156-157-93-93 0 0 250 0 0 250z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Reset axes" data-attr="zoom" data-val="reset" data-toggle="false" data-gravity="n"><svg viewBox="0 0 928.6 1000" class="icon" height="1em" width="1em"><path d="m786 296v-267q0-15-11-26t-25-10h-214v214h-143v-214h-214q-15 0-25 10t-11 26v267q0 1 0 2t0 2l321 264 321-264q1-1 1-4z m124 39l-34-41q-5-5-12-6h-2q-7 0-12 3l-386 322-386-322q-7-4-13-4-7 2-12 7l-35 41q-4 5-3 13t6 12l401 334q18 15 42 15t43-15l136-114v109q0 8 5 13t13 5h107q8 0 13-5t5-13v-227l122-102q5-5 6-12t-4-13z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Toggle Spike Lines" data-attr="_cartesianSpikesEnabled" data-val="on" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="M512 409c0-57-46-104-103-104-57 0-104 47-104 104 0 57 47 103 104 103 57 0 103-46 103-103z m-327-39l92 0 0 92-92 0z m-185 0l92 0 0 92-92 0z m370-186l92 0 0 93-92 0z m0-184l92 0 0 92-92 0z" transform="matrix(1.5 0 0 -1.5 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn active" data-title="Show closest data on hover" data-attr="hovermode" data-val="closest" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1500 1000" class="icon" height="1em" width="1em"><path d="m375 725l0 0-375-375 375-374 0-1 1125 0 0 750-1125 0z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Compare data on hover" data-attr="hovermode" data-val="x" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1125 1000" class="icon" height="1em" width="1em"><path d="m187 786l0 2-187-188 188-187 0 0 937 0 0 373-938 0z m0-499l0 1-187-188 188-188 0 0 937 0 0 376-938-1z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a href="https://plot.ly/" target="_blank" data-title="Produced with Plotly" class="modebar-btn plotlyjsicon modebar-btn--logo"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 132 132" height="1em" width="1em"><defs><style>.cls-1 {fill: #119dff;} .cls-2 {fill: #25fefd;} .cls-3 {fill: #fff;}</style></defs><title>plotly-logomark</title><g id="symbol"><rect class="cls-1" width="132" height="132" rx="6" ry="6"></rect><circle class="cls-2" cx="78" cy="54" r="6"></circle><circle class="cls-2" cx="102" cy="30" r="6"></circle><circle class="cls-2" cx="78" cy="30" r="6"></circle><circle class="cls-2" cx="54" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="54" r="6"></circle><path class="cls-3" d="M30,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,30,72Z"></path><path class="cls-3" d="M78,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,78,72Z"></path><path class="cls-3" d="M54,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,54,48Z"></path><path class="cls-3" d="M102,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,102,48Z"></path></g></svg></a></div></div></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><g class="hoverlayer"></g></svg></div></div></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';
                    
                if (document.getElementById("ed256dea-4bf4-4336-a21a-74d5a5fb4518")) {
                    Plotly.newPlot(
                        'ed256dea-4bf4-4336-a21a-74d5a5fb4518',
                        [{"marker": {"color": [0.06796144, 0.03889349, 0.07237845, 0.02628645, 0.11194395, 0.04778854, 0.05965792, 0.02774745, 0.07462718, 0.4593142, 0.01340093], "colorscale": "Portland", "showscale": true, "size": 25, "sizemode": "diameter", "sizeref": 1}, "mode": "markers", "text": ["Pclass", "Sex", "Age", "Parch", "Fare", "Embarked", "Name_length", "Has_Cabin", "FamilySize", "IsAlone", "Title"], "type": "scatter", "uid": "6ab66e1c-b36b-48b5-80ba-aafff65fbdc6", "x": ["Pclass", "Sex", "Age", "Parch", "Fare", "Embarked", "Name_length", "Has_Cabin", "FamilySize", "IsAlone", "Title"], "y": [0.06796144, 0.03889349, 0.07237845, 0.02628645, 0.11194395, 0.04778854, 0.05965792, 0.02774745, 0.07462718, 0.4593142, 0.01340093]}],
                        {"autosize": true, "hovermode": "closest", "showlegend": false, "title": {"text": "Gradient Boosting Feature Importance"}, "yaxis": {"gridwidth": 2, "ticklen": 5, "title": {"text": "Feature Importance"}}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){
                            
var gd = document.getElementById('ed256dea-4bf4-4336-a21a-74d5a5fb4518');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>이제 모든 feature importance의 평균을 계산하고 데이터프레임에 새로운 열(column)로 저장합니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[18]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># 값들의 평균을 포함하는 새 열(column)을 만듭니다.</span>

<span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'mean'</span><span class="p">]</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="o">.</span><span class="n">mean</span><span class="p">(</span><span class="n">axis</span><span class="o">=</span> <span class="mi">1</span><span class="p">)</span> <span class="c1"># axis = 1 computes the mean row-wise</span>
<span class="n">feature_dataframe</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">3</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt output_prompt">Out[18]:</div>



<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>features</th>
      <th>Random Forest feature importances</th>
      <th>Extra Trees  feature importances</th>
      <th>AdaBoost feature importances</th>
      <th>Gradient Boost feature importances</th>
      <th>mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Pclass</td>
      <td>0.104741</td>
      <td>0.121657</td>
      <td>0.028</td>
      <td>0.067961</td>
      <td>0.080590</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sex</td>
      <td>0.218370</td>
      <td>0.370983</td>
      <td>0.008</td>
      <td>0.038893</td>
      <td>0.159062</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Age</td>
      <td>0.044327</td>
      <td>0.031296</td>
      <td>0.012</td>
      <td>0.072378</td>
      <td>0.040000</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h4 id="평균-feature-importance의-Ploty-Barplot-그리기">평균 feature importance의 Ploty Barplot 그리기<a class="anchor-link" href="#평균-feature-importance의-Ploty-Barplot-그리기">¶</a></h4><p>모든 분류기에서 평균 featue importance를 얻은 다음 이를 다음과 같이 ploty가 제공하는 barplot 형태로 그릴 수 있습니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[19]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">y</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'mean'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span>
<span class="n">x</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'features'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span>
<span class="n">data</span> <span class="o">=</span> <span class="p">[</span><span class="n">go</span><span class="o">.</span><span class="n">Bar</span><span class="p">(</span>
            <span class="n">x</span><span class="o">=</span> <span class="n">x</span><span class="p">,</span>
             <span class="n">y</span><span class="o">=</span> <span class="n">y</span><span class="p">,</span>
            <span class="n">width</span> <span class="o">=</span> <span class="mf">0.5</span><span class="p">,</span>
            <span class="n">marker</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span>
               <span class="n">color</span> <span class="o">=</span> <span class="n">feature_dataframe</span><span class="p">[</span><span class="s1">'mean'</span><span class="p">]</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
            <span class="n">colorscale</span><span class="o">=</span><span class="s1">'Portland'</span><span class="p">,</span>
            <span class="n">showscale</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span>
            <span class="n">reversescale</span> <span class="o">=</span> <span class="kc">False</span>
            <span class="p">),</span>
            <span class="n">opacity</span><span class="o">=</span><span class="mf">0.6</span>
        <span class="p">)]</span>

<span class="n">layout</span><span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Layout</span><span class="p">(</span>
    <span class="n">autosize</span><span class="o">=</span> <span class="kc">True</span><span class="p">,</span>
    <span class="n">title</span><span class="o">=</span> <span class="s1">'Barplots of Mean Feature Importance'</span><span class="p">,</span>
    <span class="n">hovermode</span><span class="o">=</span> <span class="s1">'closest'</span><span class="p">,</span>
<span class="c1">#     xaxis= dict(</span>
<span class="c1">#         title= 'Pop',</span>
<span class="c1">#         ticklen= 5,</span>
<span class="c1">#         zeroline= False,</span>
<span class="c1">#         gridwidth= 2,</span>
<span class="c1">#     ),</span>
    <span class="n">yaxis</span><span class="o">=</span><span class="nb">dict</span><span class="p">(</span>
        <span class="n">title</span><span class="o">=</span> <span class="s1">'Feature Importance'</span><span class="p">,</span>
        <span class="n">ticklen</span><span class="o">=</span> <span class="mi">5</span><span class="p">,</span>
        <span class="n">gridwidth</span><span class="o">=</span> <span class="mi">2</span>
    <span class="p">),</span>
    <span class="n">showlegend</span><span class="o">=</span> <span class="kc">False</span>
<span class="p">)</span>
<span class="n">fig</span> <span class="o">=</span> <span class="n">go</span><span class="o">.</span><span class="n">Figure</span><span class="p">(</span><span class="n">data</span><span class="o">=</span><span class="n">data</span><span class="p">,</span> <span class="n">layout</span><span class="o">=</span><span class="n">layout</span><span class="p">)</span>
<span class="n">py</span><span class="o">.</span><span class="n">iplot</span><span class="p">(</span><span class="n">fig</span><span class="p">,</span> <span class="n">filename</span><span class="o">=</span><span class="s1">'bar-direct-labels'</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>



<div class="output_html rendered_html output_subarea ">
<div>
        
        
            <div id="7bbaf8f2-071b-4afc-a2e7-11737703a88d" class="plotly-graph-div js-plotly-plot" style="height:525px; width:100%;"><div class="plot-container plotly"><div class="svg-container" style="position: relative; width: 100%; height: 100%;"><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525" style="background: rgb(255, 255, 255);"><defs id="defs-d9cdc6"><g class="clips"><clipPath id="clipd9cdc6xyplot" class="plotclip"><rect width="424" height="345"></rect></clipPath><clipPath class="axesclip" id="clipd9cdc6x"><rect x="80" y="0" width="424" height="525"></rect></clipPath><clipPath class="axesclip" id="clipd9cdc6y"><rect x="0" y="100" width="602.844" height="345"></rect></clipPath><clipPath class="axesclip" id="clipd9cdc6xy"><rect x="80" y="100" width="424" height="345"></rect></clipPath></g><g class="gradients"><linearGradient x1="0" x2="0" y1="1" y2="0" id="gd9cdc6-cbeefa7b3d-a020-4a1a-84f7-41180d023d2f"><stop offset="0%" stop-color="rgb(12, 51, 131)" stop-opacity="1"></stop><stop offset="25%" stop-color="rgb(10, 136, 186)" stop-opacity="1"></stop><stop offset="50%" stop-color="rgb(242, 211, 56)" stop-opacity="1"></stop><stop offset="75%" stop-color="rgb(242, 143, 56)" stop-opacity="1"></stop><stop offset="100%" stop-color="rgb(217, 30, 30)" stop-opacity="1"></stop></linearGradient></g></defs><g class="bglayer"></g><g class="draglayer cursor-crosshair"><g class="xy"><rect class="nsewdrag drag" data-subplot="xy" x="80" y="100" width="424" height="345" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nwdrag drag cursor-nw-resize" data-subplot="xy" x="60" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nedrag drag cursor-ne-resize" data-subplot="xy" x="504" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="swdrag drag cursor-sw-resize" data-subplot="xy" x="60" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sedrag drag cursor-se-resize" data-subplot="xy" x="504" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ewdrag drag cursor-ew-resize" data-subplot="xy" x="122.4" y="445.5" width="339.20000000000005" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="wdrag drag cursor-w-resize" data-subplot="xy" x="80" y="445.5" width="42.400000000000006" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="edrag drag cursor-e-resize" data-subplot="xy" x="461.6" y="445.5" width="42.400000000000006" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nsdrag drag cursor-ns-resize" data-subplot="xy" x="59.5" y="134.5" width="20" height="276" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sdrag drag cursor-s-resize" data-subplot="xy" x="59.5" y="410.5" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ndrag drag cursor-n-resize" data-subplot="xy" x="59.5" y="100" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect></g></g><g class="layer-below"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="cartesianlayer"><g class="subplot xy"><g class="layer-subplot"><g class="shapelayer"></g><g class="imagelayer"></g></g><g class="gridlayer"><g class="x"></g><g class="y"><path class="ygrid crisp" transform="translate(0,397.62)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,350.23)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,302.85)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,255.46)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,208.07999999999998)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,160.69)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path><path class="ygrid crisp" transform="translate(0,113.31)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 2px;"></path></g></g><g class="zerolinelayer"><path class="yzl zl crisp" transform="translate(0,445)" d="M80,0h424" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path></g><path class="xlines-below"></path><path class="ylines-below"></path><g class="overlines-below"></g><g class="xaxislayer-below"></g><g class="yaxislayer-below"></g><g class="overaxes-below"></g><g class="plot" transform="translate(80, 100)" clip-path="url('#clipd9cdc6xyplot')"><g class="barlayer mlayer"><g class="trace bars" style="opacity: 0.6;"><g class="points"><g class="point"><path d="M9.64,345V268.63H28.91V345Z" style="vector-effect: non-scaling-stroke; opacity: 1; stroke-width: 0px; fill: rgb(10, 119, 175); fill-opacity: 1;"></path></g><g class="point"><path d="M48.18,345V194.26H67.45V345Z" style="vector-effect: non-scaling-stroke; opacity: 1; stroke-width: 0px; fill: rgb(183, 192, 89); fill-opacity: 1;"></path></g><g class="point"><path d="M86.73,345V307.09H106V345Z" style="vector-effect: non-scaling-stroke; opacity: 1; stroke-width: 0px; fill: rgb(11, 77, 148); fill-opacity: 1;"></path></g><g class="point"><path d="M125.27,345V315.77H144.55V345Z" style="vector-effect: non-scaling-stroke; opacity: 1; stroke-width: 0px; fill: rgb(12, 68, 142); fill-opacity: 1;"></path></g><g class="point"><path d="M163.82,345V284.93H183.09V345Z" style="vector-effect: non-scaling-stroke; opacity: 1; stroke-width: 0px; fill: rgb(11, 101, 163); fill-opacity: 1;"></path></g><g class="point"><path d="M202.36,345V318.35H221.64V345Z" style="vector-effect: non-scaling-stroke; opacity: 1; stroke-width: 0px; fill: rgb(12, 65, 140); fill-opacity: 1;"></path></g><g class="point"><path d="M240.91,345V291H260.18V345Z" style="vector-effect: non-scaling-stroke; opacity: 1; stroke-width: 0px; fill: rgb(11, 94, 159); fill-opacity: 1;"></path></g><g class="point"><path d="M279.45,345V331.04H298.73V345Z" style="vector-effect: non-scaling-stroke; opacity: 1; stroke-width: 0px; fill: rgb(12, 51, 131); fill-opacity: 1;"></path></g><g class="point"><path d="M318,345V215.33H337.27V345Z" style="vector-effect: non-scaling-stroke; opacity: 1; stroke-width: 0px; fill: rgb(120, 172, 124); fill-opacity: 1;"></path></g><g class="point"><path d="M356.55,345V17.25H375.82V345Z" style="vector-effect: non-scaling-stroke; opacity: 1; stroke-width: 0px; fill: rgb(217, 30, 30); fill-opacity: 1;"></path></g><g class="point"><path d="M395.09,345V303.67H414.36V345Z" style="vector-effect: non-scaling-stroke; opacity: 1; stroke-width: 0px; fill: rgb(11, 81, 150); fill-opacity: 1;"></path></g></g></g></g></g><g class="overplot"></g><path class="xlines-above crisp" style="fill: none;" d="M0,0"></path><path class="ylines-above crisp" style="fill: none; stroke-width: 1px; stroke: rgb(0, 0, 0); stroke-opacity: 0;" d=""></path><g class="overlines-above"></g><g class="xaxislayer-above"><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Pclass" data-math="N" transform="translate(99.27,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Pclass</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Sex" data-math="N" transform="translate(137.82,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Sex</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Age" data-math="N" transform="translate(176.36,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Age</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Parch" data-math="N" transform="translate(214.91,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Parch</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Fare" data-math="N" transform="translate(253.45,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Fare</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Embarked" data-math="N" transform="translate(292,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Embarked</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Name_length" data-math="N" transform="translate(330.55,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Name_length</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Has_Cabin" data-math="N" transform="translate(369.09,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Has_Cabin</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="FamilySize" data-math="N" transform="translate(407.64,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">FamilySize</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="IsAlone" data-math="N" transform="translate(446.18,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">IsAlone</text></g><g class="xtick"><text text-anchor="start" x="0" y="458" data-unformatted="Title" data-math="N" transform="translate(484.73,0) rotate(30,0,452)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">Title</text></g></g><g class="yaxislayer-above"><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,445)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,397.62)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,350.23)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,302.85)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,255.46)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,208.07999999999998)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,160.69)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,113.31)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0" data-math="N" transform="translate(0,445)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.05" data-math="N" transform="translate(0,397.62)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.05</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.1" data-math="N" transform="translate(0,350.23)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.1</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.15" data-math="N" transform="translate(0,302.85)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.15</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.2" data-math="N" transform="translate(0,255.46)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.2</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.25" data-math="N" transform="translate(0,208.07999999999998)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.25</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.3" data-math="N" transform="translate(0,160.69)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.3</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="0.35" data-math="N" transform="translate(0,113.31)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.35</text></g></g><g class="overaxes-above"></g></g></g><g class="polarlayer"></g><g class="ternarylayer"></g><g class="geolayer"></g><g class="funnelarealayer"></g><g class="pielayer"></g><g class="sunburstlayer"></g><g class="glimages"></g></svg><div class="gl-container"></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><defs id="topdefs-d9cdc6"><g class="clips"></g></defs><g class="indicatorlayer"></g><g class="layer-above"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="infolayer"><g class="cbeefa7b3d-a020-4a1a-84f7-41180d023d2f colorbar" transform="translate(80,100)"><rect class="cbbg" x="431.5" y="-0.5" width="78.640625" height="346" style="fill: rgb(0, 0, 0); fill-opacity: 0; stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 0;"></rect><g class="cbfills" transform="translate(0,10)"><rect class="cbfill" x="442" width="30" y="0" height="325" style="stroke: none; fill: url(&quot;#gd9cdc6-cbeefa7b3d-a020-4a1a-84f7-41180d023d2f&quot;);"></rect></g><g class="cblines" transform="translate(0,10)"></g><g class="cbaxis crisp" transform="translate(0,-100)"><g class="ycbeefa7b3d-a020-4a1a-84f7-41180d023d2ftick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.05" data-math="N" transform="translate(0,400.38)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.05</text></g><g class="ycbeefa7b3d-a020-4a1a-84f7-41180d023d2ftick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.1" data-math="N" transform="translate(0,351.31)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.1</text></g><g class="ycbeefa7b3d-a020-4a1a-84f7-41180d023d2ftick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.15" data-math="N" transform="translate(0,302.23)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.15</text></g><g class="ycbeefa7b3d-a020-4a1a-84f7-41180d023d2ftick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.2" data-math="N" transform="translate(0,253.15)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.2</text></g><g class="ycbeefa7b3d-a020-4a1a-84f7-41180d023d2ftick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.25" data-math="N" transform="translate(0,204.07)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.25</text></g><g class="ycbeefa7b3d-a020-4a1a-84f7-41180d023d2ftick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.3" data-math="N" transform="translate(0,155)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.3</text></g></g><g class="cbtitleunshift" transform="translate(-80,-100)"><g class="cbtitle"></g></g><rect class="cboutline" x="442" y="10" width="30" height="325" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; fill: none; stroke-width: 1;"></rect></g><g class="g-gtitle"><text class="gtitle" x="301.422" y="50" text-anchor="middle" dy="0em" data-unformatted="Barplots of Mean Feature Importance" data-math="N" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 17px; fill: rgb(68, 68, 68); opacity: 1; font-weight: normal; white-space: pre;">Barplots of Mean Feature Importance</text></g><g class="g-xtitle"></g><g class="g-ytitle" transform="translate(-2.65625,0)"><text class="ytitle" transform="rotate(-90,42,272.5)" x="42" y="272.5" text-anchor="middle" data-unformatted="Feature Importance" data-math="N" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 14px; fill: rgb(68, 68, 68); opacity: 1; font-weight: normal; white-space: pre;">Feature Importance</text></g></g><g class="menulayer"></g><g class="zoomlayer"></g></svg><div class="modebar-container" style="position: absolute; top: 0px; right: 0px; width: 100%;"><div id="modebar-d9cdc6" class="modebar modebar--hover ease-bg"><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Download plot as a png" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m500 450c-83 0-150-67-150-150 0-83 67-150 150-150 83 0 150 67 150 150 0 83-67 150-150 150z m400 150h-120c-16 0-34 13-39 29l-31 93c-6 15-23 28-40 28h-340c-16 0-34-13-39-28l-31-94c-6-15-23-28-40-28h-120c-55 0-100-45-100-100v-450c0-55 45-100 100-100h800c55 0 100 45 100 100v450c0 55-45 100-100 100z m-400-550c-138 0-250 112-250 250 0 138 112 250 250 250 138 0 250-112 250-250 0-138-112-250-250-250z m365 380c-19 0-35 16-35 35 0 19 16 35 35 35 19 0 35-16 35-35 0-19-16-35-35-35z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn active" data-title="Zoom" data-attr="dragmode" data-val="zoom" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000-25l-250 251c40 63 63 138 63 218 0 224-182 406-407 406-224 0-406-182-406-406s183-406 407-406c80 0 155 22 218 62l250-250 125 125z m-812 250l0 438 437 0 0-438-437 0z m62 375l313 0 0-312-313 0 0 312z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Pan" data-attr="dragmode" data-val="pan" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000 350l-187 188 0-125-250 0 0 250 125 0-188 187-187-187 125 0 0-250-250 0 0 125-188-188 186-187 0 125 252 0 0-250-125 0 187-188 188 188-125 0 0 250 250 0 0-126 187 188z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Box Select" data-attr="dragmode" data-val="select" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m0 850l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m285 0l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m-857-286l0-143 143 0 0 143-143 0z m857 0l0-143 143 0 0 143-143 0z m-857-285l0-143 143 0 0 143-143 0z m857 0l0-143 143 0 0 143-143 0z m-857-286l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z m285 0l0-143 143 0 0 143-143 0z m286 0l0-143 143 0 0 143-143 0z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Lasso Select" data-attr="dragmode" data-val="lasso" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1031 1000" class="icon" height="1em" width="1em"><path d="m1018 538c-36 207-290 336-568 286-277-48-473-256-436-463 10-57 36-108 76-151-13-66 11-137 68-183 34-28 75-41 114-42l-55-70 0 0c-2-1-3-2-4-3-10-14-8-34 5-45 14-11 34-8 45 4 1 1 2 3 2 5l0 0 113 140c16 11 31 24 45 40 4 3 6 7 8 11 48-3 100 0 151 9 278 48 473 255 436 462z m-624-379c-80 14-149 48-197 96 42 42 109 47 156 9 33-26 47-66 41-105z m-187-74c-19 16-33 37-39 60 50-32 109-55 174-68-42-25-95-24-135 8z m360 75c-34-7-69-9-102-8 8 62-16 128-68 170-73 59-175 54-244-5-9 20-16 40-20 61-28 159 121 317 333 354s407-60 434-217c28-159-121-318-333-355z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Zoom in" data-attr="zoom" data-val="in" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m1 787l0-875 875 0 0 875-875 0z m687-500l-187 0 0-187-125 0 0 187-188 0 0 125 188 0 0 187 125 0 0-187 187 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Zoom out" data-attr="zoom" data-val="out" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m0 788l0-876 875 0 0 876-875 0z m688-500l-500 0 0 125 500 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Autoscale" data-attr="zoom" data-val="auto" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m250 850l-187 0-63 0 0-62 0-188 63 0 0 188 187 0 0 62z m688 0l-188 0 0-62 188 0 0-188 62 0 0 188 0 62-62 0z m-875-938l0 188-63 0 0-188 0-62 63 0 187 0 0 62-187 0z m875 188l0-188-188 0 0-62 188 0 62 0 0 62 0 188-62 0z m-125 188l-1 0-93-94-156 156 156 156 92-93 2 0 0 250-250 0 0-2 93-92-156-156-156 156 94 92 0 2-250 0 0-250 0 0 93 93 157-156-157-156-93 94 0 0 0-250 250 0 0 0-94 93 156 157 156-157-93-93 0 0 250 0 0 250z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Reset axes" data-attr="zoom" data-val="reset" data-toggle="false" data-gravity="n"><svg viewBox="0 0 928.6 1000" class="icon" height="1em" width="1em"><path d="m786 296v-267q0-15-11-26t-25-10h-214v214h-143v-214h-214q-15 0-25 10t-11 26v267q0 1 0 2t0 2l321 264 321-264q1-1 1-4z m124 39l-34-41q-5-5-12-6h-2q-7 0-12 3l-386 322-386-322q-7-4-13-4-7 2-12 7l-35 41q-4 5-3 13t6 12l401 334q18 15 42 15t43-15l136-114v109q0 8 5 13t13 5h107q8 0 13-5t5-13v-227l122-102q5-5 6-12t-4-13z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Toggle Spike Lines" data-attr="_cartesianSpikesEnabled" data-val="on" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="M512 409c0-57-46-104-103-104-57 0-104 47-104 104 0 57 47 103 104 103 57 0 103-46 103-103z m-327-39l92 0 0 92-92 0z m-185 0l92 0 0 92-92 0z m370-186l92 0 0 93-92 0z m0-184l92 0 0 92-92 0z" transform="matrix(1.5 0 0 -1.5 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn active" data-title="Show closest data on hover" data-attr="hovermode" data-val="closest" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1500 1000" class="icon" height="1em" width="1em"><path d="m375 725l0 0-375-375 375-374 0-1 1125 0 0 750-1125 0z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Compare data on hover" data-attr="hovermode" data-val="x" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1125 1000" class="icon" height="1em" width="1em"><path d="m187 786l0 2-187-188 188-187 0 0 937 0 0 373-938 0z m0-499l0 1-187-188 188-188 0 0 937 0 0 376-938-1z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a href="https://plot.ly/" target="_blank" data-title="Produced with Plotly" class="modebar-btn plotlyjsicon modebar-btn--logo"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 132 132" height="1em" width="1em"><defs><style>.cls-1 {fill: #119dff;} .cls-2 {fill: #25fefd;} .cls-3 {fill: #fff;}</style></defs><title>plotly-logomark</title><g id="symbol"><rect class="cls-1" width="132" height="132" rx="6" ry="6"></rect><circle class="cls-2" cx="78" cy="54" r="6"></circle><circle class="cls-2" cx="102" cy="30" r="6"></circle><circle class="cls-2" cx="78" cy="30" r="6"></circle><circle class="cls-2" cx="54" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="54" r="6"></circle><path class="cls-3" d="M30,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,30,72Z"></path><path class="cls-3" d="M78,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,78,72Z"></path><path class="cls-3" d="M54,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,54,48Z"></path><path class="cls-3" d="M102,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,102,48Z"></path></g></svg></a></div></div></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><g class="hoverlayer"></g></svg></div></div></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';
                    
                if (document.getElementById("7bbaf8f2-071b-4afc-a2e7-11737703a88d")) {
                    Plotly.newPlot(
                        '7bbaf8f2-071b-4afc-a2e7-11737703a88d',
                        [{"marker": {"color": [0.08058984, 0.1590617125, 0.0400003, 0.030840205, 0.0633819925, 0.028122312499999996, 0.0569813925, 0.0147330775, 0.1368327725, 0.34584334250000004, 0.0436130625], "colorscale": "Portland", "reversescale": false, "showscale": true}, "opacity": 0.6, "type": "bar", "uid": "eefa7b3d-a020-4a1a-84f7-41180d023d2f", "width": 0.5, "x": ["Pclass", "Sex", "Age", "Parch", "Fare", "Embarked", "Name_length", "Has_Cabin", "FamilySize", "IsAlone", "Title"], "y": [0.08058984, 0.1590617125, 0.0400003, 0.030840205, 0.0633819925, 0.028122312499999996, 0.0569813925, 0.0147330775, 0.1368327725, 0.34584334250000004, 0.0436130625]}],
                        {"autosize": true, "hovermode": "closest", "showlegend": false, "title": {"text": "Barplots of Mean Feature Importance"}, "yaxis": {"gridwidth": 2, "ticklen": 5, "title": {"text": "Feature Importance"}}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){
                            
var gd = document.getElementById('7bbaf8f2-071b-4afc-a2e7-11737703a88d');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="첫번째-단계의-예측값들을-통해-두-번째-단계의-예측값-뽑기">첫번째 단계의 예측값들을 통해 두 번째 단계의 예측값 뽑기<a class="anchor-link" href="#첫번째-단계의-예측값들을-통해-두-번째-단계의-예측값-뽑기">¶</a></h2><h4 id="첫-번째-단계-결과물을-새로운-feature로서-사용하기">첫 번째 단계 결과물을 새로운 feature로서 사용하기<a class="anchor-link" href="#첫-번째-단계-결과물을-새로운-feature로서-사용하기">¶</a></h4><p>첫 번째 단계로부터 예측값들을 얻었으므로 이제 다음 분류기들을 위한 새로운 feature들의 집합을 구축하는 것을 생각해 볼 수 있습니다. 아래 코드처럼 이전 분류기들로부터 첫 번째 단계의 예측값들을 새 열(column)로 가져오고 이를 통해 다음 분류기들을 학습할 수 있습니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[20]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">base_predictions_train</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">DataFrame</span><span class="p">(</span> <span class="p">{</span><span class="s1">'RandomForest'</span><span class="p">:</span> <span class="n">rf_oof_train</span><span class="o">.</span><span class="n">ravel</span><span class="p">(),</span>
     <span class="s1">'ExtraTrees'</span><span class="p">:</span> <span class="n">et_oof_train</span><span class="o">.</span><span class="n">ravel</span><span class="p">(),</span>
     <span class="s1">'AdaBoost'</span><span class="p">:</span> <span class="n">ada_oof_train</span><span class="o">.</span><span class="n">ravel</span><span class="p">(),</span>
      <span class="s1">'GradientBoost'</span><span class="p">:</span> <span class="n">gb_oof_train</span><span class="o">.</span><span class="n">ravel</span><span class="p">()</span>
    <span class="p">})</span>
<span class="n">base_predictions_train</span><span class="o">.</span><span class="n">head</span><span class="p">()</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt output_prompt">Out[20]:</div>



<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>RandomForest</th>
      <th>ExtraTrees</th>
      <th>AdaBoost</th>
      <th>GradientBoost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h4 id="두-번째-단계의-train-세트에-대한-상관관계-히트맵(correlation-heatmap)-그려보기">두 번째 단계의 train 세트에 대한 상관관계 히트맵(correlation heatmap) 그려보기<a class="anchor-link" href="#두-번째-단계의-train-세트에-대한-상관관계-히트맵(correlation-heatmap)-그려보기">¶</a></h4>
</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[21]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">data</span> <span class="o">=</span> <span class="p">[</span>
    <span class="n">go</span><span class="o">.</span><span class="n">Heatmap</span><span class="p">(</span>
        <span class="n">z</span><span class="o">=</span> <span class="n">base_predictions_train</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="nb">float</span><span class="p">)</span><span class="o">.</span><span class="n">corr</span><span class="p">()</span><span class="o">.</span><span class="n">values</span> <span class="p">,</span>
        <span class="n">x</span><span class="o">=</span><span class="n">base_predictions_train</span><span class="o">.</span><span class="n">columns</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
        <span class="n">y</span><span class="o">=</span> <span class="n">base_predictions_train</span><span class="o">.</span><span class="n">columns</span><span class="o">.</span><span class="n">values</span><span class="p">,</span>
          <span class="n">colorscale</span><span class="o">=</span><span class="s1">'Viridis'</span><span class="p">,</span>
            <span class="n">showscale</span><span class="o">=</span><span class="kc">True</span><span class="p">,</span>
            <span class="n">reversescale</span> <span class="o">=</span> <span class="kc">True</span>
    <span class="p">)</span>
<span class="p">]</span>
<span class="n">py</span><span class="o">.</span><span class="n">iplot</span><span class="p">(</span><span class="n">data</span><span class="p">,</span> <span class="n">filename</span><span class="o">=</span><span class="s1">'labelled-heatmap'</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area">

    <div class="prompt"></div>



<div class="output_html rendered_html output_subarea ">
<div>
        
        
            <div id="94f9851d-f2b8-44be-af81-f31155531aa1" class="plotly-graph-div js-plotly-plot" style="height:525px; width:100%;"><div class="plot-container plotly"><div class="svg-container" style="position: relative; width: 100%; height: 100%;"><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525" style="background: rgb(255, 255, 255);"><defs id="defs-4cfd16"><g class="clips"><clipPath id="clip4cfd16xyplot" class="plotclip"><rect width="424" height="345"></rect></clipPath><clipPath class="axesclip" id="clip4cfd16x"><rect x="80" y="0" width="424" height="525"></rect></clipPath><clipPath class="axesclip" id="clip4cfd16y"><rect x="0" y="100" width="602.844" height="345"></rect></clipPath><clipPath class="axesclip" id="clip4cfd16xy"><rect x="80" y="100" width="424" height="345"></rect></clipPath></g><g class="gradients"><linearGradient x1="0" x2="0" y1="1" y2="0" id="g4cfd16-cb117ebd26-9bf1-4f54-9b19-6e7919a591da"><stop offset="0%" stop-color="rgb(253, 231, 37)" stop-opacity="1"></stop><stop offset="5.882352941176472%" stop-color="rgb(216, 226, 25)" stop-opacity="1"></stop><stop offset="12.15686274509804%" stop-color="rgb(173, 220, 48)" stop-opacity="1"></stop><stop offset="18.43137254901961%" stop-color="rgb(132, 212, 75)" stop-opacity="1"></stop><stop offset="24.705882352941178%" stop-color="rgb(94, 201, 98)" stop-opacity="1"></stop><stop offset="30.980392156862745%" stop-color="rgb(63, 188, 115)" stop-opacity="1"></stop><stop offset="37.254901960784316%" stop-color="rgb(40, 174, 128)" stop-opacity="1"></stop><stop offset="43.529411764705884%" stop-color="rgb(31, 160, 136)" stop-opacity="1"></stop><stop offset="49.80392156862745%" stop-color="rgb(33, 145, 140)" stop-opacity="1"></stop><stop offset="56.07843137254902%" stop-color="rgb(38, 130, 142)" stop-opacity="1"></stop><stop offset="62.35294117647059%" stop-color="rgb(44, 114, 142)" stop-opacity="1"></stop><stop offset="68.62745098039215%" stop-color="rgb(51, 99, 141)" stop-opacity="1"></stop><stop offset="74.90196078431373%" stop-color="rgb(59, 82, 139)" stop-opacity="1"></stop><stop offset="81.17647058823529%" stop-color="rgb(66, 64, 134)" stop-opacity="1"></stop><stop offset="87.45098039215686%" stop-color="rgb(71, 45, 123)" stop-opacity="1"></stop><stop offset="93.72549019607843%" stop-color="rgb(72, 24, 106)" stop-opacity="1"></stop><stop offset="100%" stop-color="rgb(68, 1, 84)" stop-opacity="1"></stop></linearGradient></g></defs><g class="bglayer"></g><g class="draglayer cursor-crosshair"><g class="xy"><rect class="nsewdrag drag" data-subplot="xy" x="80" y="100" width="424" height="345" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nwdrag drag cursor-nw-resize" data-subplot="xy" x="60" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nedrag drag cursor-ne-resize" data-subplot="xy" x="504" y="80" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="swdrag drag cursor-sw-resize" data-subplot="xy" x="60" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sedrag drag cursor-se-resize" data-subplot="xy" x="504" y="445" width="20" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ewdrag drag cursor-ew-resize" data-subplot="xy" x="122.4" y="445.5" width="339.20000000000005" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="wdrag drag cursor-w-resize" data-subplot="xy" x="80" y="445.5" width="42.400000000000006" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="edrag drag cursor-e-resize" data-subplot="xy" x="461.6" y="445.5" width="42.400000000000006" height="20" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="nsdrag drag cursor-ns-resize" data-subplot="xy" x="59.5" y="134.5" width="20" height="276" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="sdrag drag cursor-s-resize" data-subplot="xy" x="59.5" y="410.5" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect><rect class="ndrag drag cursor-n-resize" data-subplot="xy" x="59.5" y="100" width="20" height="34.5" style="fill: transparent; stroke-width: 0; pointer-events: all;"></rect></g></g><g class="layer-below"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="cartesianlayer"><g class="subplot xy"><g class="layer-subplot"><g class="shapelayer"></g><g class="imagelayer"></g></g><g class="gridlayer"><g class="x"><path class="xgrid crisp" transform="translate(133,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(239,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(345,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xgrid crisp" transform="translate(451,0)" d="M0,100v345" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path></g><g class="y"><path class="ygrid crisp" transform="translate(0,401.88)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ygrid crisp" transform="translate(0,315.63)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ygrid crisp" transform="translate(0,229.38)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ygrid crisp" transform="translate(0,143.13)" d="M80,0h424" style="stroke: rgb(238, 238, 238); stroke-opacity: 1; stroke-width: 1px;"></path></g></g><g class="zerolinelayer"></g><path class="xlines-below"></path><path class="ylines-below"></path><g class="overlines-below"></g><g class="xaxislayer-below"></g><g class="yaxislayer-below"></g><g class="overaxes-below"></g><g class="plot" transform="translate(80, 100)" clip-path="url('#clip4cfd16xyplot')"><g class="heatmaplayer mlayer"><g class="hm"><image xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="none" height="345" width="424" x="0" y="0" xlink:href="data:image/png;base64,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CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6BRQRqeAQjoFlBEqoIxOAWV0CiikU0AZoQLK6LQcLicrY+rcK9/O9uc+APL65eAdWTr3zOOtp7kPgLw+oJBQAWWECiijU0AZnQLK6DQCyggVUEangDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo1NAGZ0CCukUUEaogDI6BZTRKaCQTgFlhAooo9N/0gm8HDWp/Y4AAAAASUVORK5CYII=" style="opacity: 1;"></image></g></g></g><g class="overplot"></g><path class="xlines-above crisp" style="fill: none; stroke-width: 1px; stroke: rgb(0, 0, 0); stroke-opacity: 0;" d=""></path><path class="ylines-above crisp" style="fill: none; stroke-width: 1px; stroke: rgb(0, 0, 0); stroke-opacity: 0;" d=""></path><g class="overlines-above"></g><g class="xaxislayer-above"><path class="xtick ticks crisp" d="M0,446v5" transform="translate(133,0)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xtick ticks crisp" d="M0,446v5" transform="translate(239,0)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xtick ticks crisp" d="M0,446v5" transform="translate(345,0)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="xtick ticks crisp" d="M0,446v5" transform="translate(451,0)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><g class="xtick"><text text-anchor="middle" x="0" y="465.4" data-unformatted="RandomForest" data-math="N" transform="translate(133,0)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">RandomForest</text></g><g class="xtick"><text text-anchor="middle" x="0" y="465.4" data-unformatted="ExtraTrees" data-math="N" transform="translate(239,0)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">ExtraTrees</text></g><g class="xtick"><text text-anchor="middle" x="0" y="465.4" data-unformatted="AdaBoost" data-math="N" transform="translate(345,0)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">AdaBoost</text></g><g class="xtick"><text text-anchor="middle" x="0" y="465.4" data-unformatted="GradientBoost" data-math="N" transform="translate(451,0)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">GradientBoost</text></g></g><g class="yaxislayer-above"><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,401.88)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,315.63)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,229.38)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><path class="ytick ticks crisp" d="M79,0h-5" transform="translate(0,143.13)" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 1px;"></path><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="RandomForest" data-math="N" transform="translate(0,401.88)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">RandomForest</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="ExtraTrees" data-math="N" transform="translate(0,315.63)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">ExtraTrees</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="AdaBoost" data-math="N" transform="translate(0,229.38)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">AdaBoost</text></g><g class="ytick"><text text-anchor="end" x="71.6" y="4.199999999999999" data-unformatted="GradientBoost" data-math="N" transform="translate(0,143.13)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">GradientBoost</text></g></g><g class="overaxes-above"></g></g></g><g class="polarlayer"></g><g class="ternarylayer"></g><g class="geolayer"></g><g class="funnelarealayer"></g><g class="pielayer"></g><g class="sunburstlayer"></g><g class="glimages"></g></svg><div class="gl-container"></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><defs id="topdefs-4cfd16"><g class="clips"></g></defs><g class="indicatorlayer"></g><g class="layer-above"><g class="imagelayer"></g><g class="shapelayer"></g></g><g class="infolayer"><g class="cb117ebd26-9bf1-4f54-9b19-6e7919a591da colorbar" transform="translate(80,100)"><rect class="cbbg" x="431.5" y="-0.5" width="78.640625" height="346" style="fill: rgb(0, 0, 0); fill-opacity: 0; stroke: rgb(68, 68, 68); stroke-opacity: 1; stroke-width: 0;"></rect><g class="cbfills" transform="translate(0,10)"><rect class="cbfill" x="442" width="30" y="0" height="325" style="stroke: none; fill: url(&quot;#g4cfd16-cb117ebd26-9bf1-4f54-9b19-6e7919a591da&quot;);"></rect></g><g class="cblines" transform="translate(0,10)"></g><g class="cbaxis crisp" transform="translate(0,-100)"><g class="ycb117ebd26-9bf1-4f54-9b19-6e7919a591datick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.7" data-math="N" transform="translate(0,399.42)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.7</text></g><g class="ycb117ebd26-9bf1-4f54-9b19-6e7919a591datick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.75" data-math="N" transform="translate(0,351.18)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.75</text></g><g class="ycb117ebd26-9bf1-4f54-9b19-6e7919a591datick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.8" data-math="N" transform="translate(0,302.95)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.8</text></g><g class="ycb117ebd26-9bf1-4f54-9b19-6e7919a591datick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.85" data-math="N" transform="translate(0,254.71)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.85</text></g><g class="ycb117ebd26-9bf1-4f54-9b19-6e7919a591datick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.9" data-math="N" transform="translate(0,206.47)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.9</text></g><g class="ycb117ebd26-9bf1-4f54-9b19-6e7919a591datick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="0.95" data-math="N" transform="translate(0,158.24)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">0.95</text></g><g class="ycb117ebd26-9bf1-4f54-9b19-6e7919a591datick"><text text-anchor="start" x="475.4" y="4.199999999999999" data-unformatted="1" data-math="N" transform="translate(0,110)" style="font-family: &quot;Open Sans&quot;, verdana, arial, sans-serif; font-size: 12px; fill: rgb(68, 68, 68); fill-opacity: 1; white-space: pre;">1</text></g></g><g class="cbtitleunshift" transform="translate(-80,-100)"><g class="cbtitle"></g></g><rect class="cboutline" x="442" y="10" width="30" height="325" style="stroke: rgb(68, 68, 68); stroke-opacity: 1; fill: none; stroke-width: 1;"></rect></g><g class="g-gtitle"></g><g class="g-xtitle"></g><g class="g-ytitle"></g></g><g class="menulayer"></g><g class="zoomlayer"></g></svg><div class="modebar-container" style="position: absolute; top: 0px; right: 0px; width: 100%;"><div id="modebar-4cfd16" class="modebar modebar--hover ease-bg"><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Download plot as a png" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m500 450c-83 0-150-67-150-150 0-83 67-150 150-150 83 0 150 67 150 150 0 83-67 150-150 150z m400 150h-120c-16 0-34 13-39 29l-31 93c-6 15-23 28-40 28h-340c-16 0-34-13-39-28l-31-94c-6-15-23-28-40-28h-120c-55 0-100-45-100-100v-450c0-55 45-100 100-100h800c55 0 100 45 100 100v450c0 55-45 100-100 100z m-400-550c-138 0-250 112-250 250 0 138 112 250 250 250 138 0 250-112 250-250 0-138-112-250-250-250z m365 380c-19 0-35 16-35 35 0 19 16 35 35 35 19 0 35-16 35-35 0-19-16-35-35-35z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn active" data-title="Zoom" data-attr="dragmode" data-val="zoom" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000-25l-250 251c40 63 63 138 63 218 0 224-182 406-407 406-224 0-406-182-406-406s183-406 407-406c80 0 155 22 218 62l250-250 125 125z m-812 250l0 438 437 0 0-438-437 0z m62 375l313 0 0-312-313 0 0 312z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Pan" data-attr="dragmode" data-val="pan" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m1000 350l-187 188 0-125-250 0 0 250 125 0-188 187-187-187 125 0 0-250-250 0 0 125-188-188 186-187 0 125 252 0 0-250-125 0 187-188 188 188-125 0 0 250 250 0 0-126 187 188z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Zoom in" data-attr="zoom" data-val="in" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m1 787l0-875 875 0 0 875-875 0z m687-500l-187 0 0-187-125 0 0 187-188 0 0 125 188 0 0 187 125 0 0-187 187 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Zoom out" data-attr="zoom" data-val="out" data-toggle="false" data-gravity="n"><svg viewBox="0 0 875 1000" class="icon" height="1em" width="1em"><path d="m0 788l0-876 875 0 0 876-875 0z m688-500l-500 0 0 125 500 0 0-125z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Autoscale" data-attr="zoom" data-val="auto" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="m250 850l-187 0-63 0 0-62 0-188 63 0 0 188 187 0 0 62z m688 0l-188 0 0-62 188 0 0-188 62 0 0 188 0 62-62 0z m-875-938l0 188-63 0 0-188 0-62 63 0 187 0 0 62-187 0z m875 188l0-188-188 0 0-62 188 0 62 0 0 62 0 188-62 0z m-125 188l-1 0-93-94-156 156 156 156 92-93 2 0 0 250-250 0 0-2 93-92-156-156-156 156 94 92 0 2-250 0 0-250 0 0 93 93 157-156-157-156-93 94 0 0 0-250 250 0 0 0-94 93 156 157 156-157-93-93 0 0 250 0 0 250z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Reset axes" data-attr="zoom" data-val="reset" data-toggle="false" data-gravity="n"><svg viewBox="0 0 928.6 1000" class="icon" height="1em" width="1em"><path d="m786 296v-267q0-15-11-26t-25-10h-214v214h-143v-214h-214q-15 0-25 10t-11 26v267q0 1 0 2t0 2l321 264 321-264q1-1 1-4z m124 39l-34-41q-5-5-12-6h-2q-7 0-12 3l-386 322-386-322q-7-4-13-4-7 2-12 7l-35 41q-4 5-3 13t6 12l401 334q18 15 42 15t43-15l136-114v109q0 8 5 13t13 5h107q8 0 13-5t5-13v-227l122-102q5-5 6-12t-4-13z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a rel="tooltip" class="modebar-btn" data-title="Toggle Spike Lines" data-attr="_cartesianSpikesEnabled" data-val="on" data-toggle="false" data-gravity="n"><svg viewBox="0 0 1000 1000" class="icon" height="1em" width="1em"><path d="M512 409c0-57-46-104-103-104-57 0-104 47-104 104 0 57 47 103 104 103 57 0 103-46 103-103z m-327-39l92 0 0 92-92 0z m-185 0l92 0 0 92-92 0z m370-186l92 0 0 93-92 0z m0-184l92 0 0 92-92 0z" transform="matrix(1.5 0 0 -1.5 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn" data-title="Show closest data on hover" data-attr="hovermode" data-val="closest" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1500 1000" class="icon" height="1em" width="1em"><path d="m375 725l0 0-375-375 375-374 0-1 1125 0 0 750-1125 0z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a><a rel="tooltip" class="modebar-btn active" data-title="Compare data on hover" data-attr="hovermode" data-val="x" data-toggle="false" data-gravity="ne"><svg viewBox="0 0 1125 1000" class="icon" height="1em" width="1em"><path d="m187 786l0 2-187-188 188-187 0 0 937 0 0 373-938 0z m0-499l0 1-187-188 188-188 0 0 937 0 0 376-938-1z" transform="matrix(1 0 0 -1 0 850)"></path></svg></a></div><div class="modebar-group"><a href="https://plot.ly/" target="_blank" data-title="Produced with Plotly" class="modebar-btn plotlyjsicon modebar-btn--logo"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 132 132" height="1em" width="1em"><defs><style>.cls-1 {fill: #119dff;} .cls-2 {fill: #25fefd;} .cls-3 {fill: #fff;}</style></defs><title>plotly-logomark</title><g id="symbol"><rect class="cls-1" width="132" height="132" rx="6" ry="6"></rect><circle class="cls-2" cx="78" cy="54" r="6"></circle><circle class="cls-2" cx="102" cy="30" r="6"></circle><circle class="cls-2" cx="78" cy="30" r="6"></circle><circle class="cls-2" cx="54" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="30" r="6"></circle><circle class="cls-2" cx="30" cy="54" r="6"></circle><path class="cls-3" d="M30,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,30,72Z"></path><path class="cls-3" d="M78,72a6,6,0,0,0-6,6v24a6,6,0,0,0,12,0V78A6,6,0,0,0,78,72Z"></path><path class="cls-3" d="M54,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,54,48Z"></path><path class="cls-3" d="M102,48a6,6,0,0,0-6,6v48a6,6,0,0,0,12,0V54A6,6,0,0,0,102,48Z"></path></g></svg></a></div></div></div><svg class="main-svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="602.844" height="525"><g class="hoverlayer"></g></svg></div></div></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';
                    
                if (document.getElementById("94f9851d-f2b8-44be-af81-f31155531aa1")) {
                    Plotly.newPlot(
                        '94f9851d-f2b8-44be-af81-f31155531aa1',
                        [{"colorscale": "Viridis", "reversescale": true, "showscale": true, "type": "heatmap", "uid": "117ebd26-9bf1-4f54-9b19-6e7919a591da", "x": ["RandomForest", "ExtraTrees", "AdaBoost", "GradientBoost"], "y": ["RandomForest", "ExtraTrees", "AdaBoost", "GradientBoost"], "z": [[1.0, 0.8755416342429254, 0.7828559228783493, 0.7286294836301596], [0.8755416342429254, 1.0, 0.7895054507871532, 0.7215038347699684], [0.7828559228783493, 0.7895054507871532, 1.0, 0.663117156765499], [0.7286294836301596, 0.7215038347699684, 0.663117156765499, 1.0]]}],
                        {},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){
                            
var gd = document.getElementById('94f9851d-f2b8-44be-af81-f31155531aa1');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>서로 관련성이 없는 훈련된 모델들을 사용하면 더 좋은 점수를 얻는 것에 유리하다는 기사들과 Kaggle 우승자들의 후기가 있습니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[22]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">x_train</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">concatenate</span><span class="p">((</span> <span class="n">et_oof_train</span><span class="p">,</span> <span class="n">rf_oof_train</span><span class="p">,</span> <span class="n">ada_oof_train</span><span class="p">,</span> <span class="n">gb_oof_train</span><span class="p">,</span> <span class="n">svc_oof_train</span><span class="p">),</span> <span class="n">axis</span><span class="o">=</span><span class="mi">1</span><span class="p">)</span>
<span class="n">x_test</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">concatenate</span><span class="p">((</span> <span class="n">et_oof_test</span><span class="p">,</span> <span class="n">rf_oof_test</span><span class="p">,</span> <span class="n">ada_oof_test</span><span class="p">,</span> <span class="n">gb_oof_test</span><span class="p">,</span> <span class="n">svc_oof_test</span><span class="p">),</span> <span class="n">axis</span><span class="o">=</span><span class="mi">1</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>첫 번째 단계의 train과 test 예측값들을 연결(concatenate)하여 x_train과 x_test로 연결했으므로, 이제 우리는 두 번째 단계의 학습 모델들의 학습을 진행해 볼 수 있습니다.</p>
<h3 id="XGBoost-분류기를-이용한-두-번째-단계-모델-학습">XGBoost 분류기를 이용한 두 번째 단계 모델 학습<a class="anchor-link" href="#XGBoost-분류기를-이용한-두-번째-단계-모델-학습">¶</a></h3><p>이 단계에서 우리는 boosted tree 학습 모델을 위한 매우 유명한 라이브러리인 XGBoost를 선택할 것입니다. XGBoost는 대규모의 boosted tree 알고리즘을 위해 개발되었습니다. 알고리즘에 대한 자세한 설명은 공식 문서를 확인하십시오.</p>
<p>어쨌든 우리는 XGBClassifier를 호출하고 첫 번째 단계의 train과 target 데이터에 적합(fit)시킬 것입니다. 그리고 학습이 끝난 모델로 다음과 같이 예측을 수행할 것입니다:</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[23]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">gbm</span> <span class="o">=</span> <span class="n">xgb</span><span class="o">.</span><span class="n">XGBClassifier</span><span class="p">(</span>
    <span class="c1">#learning_rate = 0.02,</span>
 <span class="n">n_estimators</span><span class="o">=</span> <span class="mi">2000</span><span class="p">,</span>
 <span class="n">max_depth</span><span class="o">=</span> <span class="mi">4</span><span class="p">,</span>
 <span class="n">min_child_weight</span><span class="o">=</span> <span class="mi">2</span><span class="p">,</span>
 <span class="c1">#gamma=1,</span>
 <span class="n">gamma</span><span class="o">=</span><span class="mf">0.9</span><span class="p">,</span>                        
 <span class="n">subsample</span><span class="o">=</span><span class="mf">0.8</span><span class="p">,</span>
 <span class="n">colsample_bytree</span><span class="o">=</span><span class="mf">0.8</span><span class="p">,</span>
 <span class="n">objective</span><span class="o">=</span> <span class="s1">'binary:logistic'</span><span class="p">,</span>
 <span class="n">nthread</span><span class="o">=</span> <span class="o">-</span><span class="mi">1</span><span class="p">,</span>
 <span class="n">scale_pos_weight</span><span class="o">=</span><span class="mi">1</span><span class="p">)</span><span class="o">.</span><span class="n">fit</span><span class="p">(</span><span class="n">x_train</span><span class="p">,</span> <span class="n">y_train</span><span class="p">)</span>
<span class="n">predictions</span> <span class="o">=</span> <span class="n">gbm</span><span class="o">.</span><span class="n">predict</span><span class="p">(</span><span class="n">x_test</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>모델에서 사용된 XGBoost 매개 변수들에 대해 빠르게 설명하겠습니다.</p>
<ul>
<li>max_depth : tree를 얼마나 키울지 결정합니다. 지나치게 높으면 과적합을 유발합니다.</li>
<li>gamma : tree의 leaf 노드(맨 마지막)에서 추가 파티션을 만드는 데 필요한 minimum loss reduction입니다. 클수록 더 보수적인(안전한) 알고리즘이 될 것입니다. </li>
<li>eta : 과적합(overfitting)을 방지하기 위해 각 부스팅 단계에서 사용되는 스텝 사이즈 감소량입니다. </li>
</ul>
<h4 id="제출을-위한-파일-생성하기">제출을 위한 파일 생성하기<a class="anchor-link" href="#제출을-위한-파일-생성하기">¶</a></h4><p>마지막으로, 훈련된 모든 첫 번째 단계 및 두 번째 단계의 모델을 통해 Titanic 대회에 제출할 적절한 형식으로 예측값을 출력해 봅니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[24]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Generate Submission File </span>
<span class="n">StackingSubmission</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">DataFrame</span><span class="p">({</span> <span class="s1">'PassengerId'</span><span class="p">:</span> <span class="n">PassengerId</span><span class="p">,</span>
                            <span class="s1">'Survived'</span><span class="p">:</span> <span class="n">predictions</span> <span class="p">})</span>
<span class="n">StackingSubmission</span><span class="o">.</span><span class="n">to_csv</span><span class="p">(</span><span class="s2">"StackingSubmission.csv"</span><span class="p">,</span> <span class="n">index</span><span class="o">=</span><span class="kc">False</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered"><div class="prompt input_prompt">
</div><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h4 id="추가-개선-단계">추가 개선 단계<a class="anchor-link" href="#추가-개선-단계">¶</a></h4><p>지금까지 앙상블 스태커를 제작하는 아주 간단한 방법을 보여 주었습니다. Kaggle 대회에서 가장 높은 순위에서는 괴상한 stacked classifiers의 조합 뿐만 아니라 두 단계 이상까지도 가는 스태킹도 사용하고 있습니다.</p>
<p>점수를 향상시키기 위해 할 수 있는 몇 가지 추가 단계는 다음과 같습니다:</p>
<ul>
<li><ol>
<li>최적의 매개 변수 값을 찾기 위해 모델을 학습할 때 Cross-validation(교차 검증)을 사용할 수도 있습니다. </li>
</ol>
</li>
<li><ol>
<li>학습에 보다 다양한 기본 모델들을 사용하십시오. 결과가 더 관련성이 없을수록 더 좋은 최종 결과를 보여줍니다. </li>
</ol>
</li>
</ul>
<h3 id="결론">결론<a class="anchor-link" href="#결론">¶</a></h3><p>이 노트북은 학습 모델들을 쌓기(stacking) 위한 스크립트를 소개하고 있습니다. 다시한번 Faron과 Sina에게 고마움을 표합니다.</p>
<p>스태킹이나 앙상블 기법에 대한 다른 훌륭한 자료는 MLWave:<a href="http://mlwave.com/kaggle-ensembling-guide/">Kaggle Ensembling Guide</a>를 참고하십시오.</p>
<p>다음에 또 뵙겠습니다.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

    </div>
</div>
</div>

</div>
    </div>
  </div>


 



<svg id="js-plotly-tester" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" style="position: absolute; left: -10000px; top: -10000px; width: 9000px; height: 9000px; z-index: 1;"><path class="js-reference-point" d="M0,0H1V1H0Z" style="stroke-width: 0; fill: black;"></path></svg></body></html>