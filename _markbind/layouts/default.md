<head-bottom>
  <link rel="stylesheet" href="{{baseUrl}}/stylesheets/main.css">
</head-bottom>

<header fixed>
<navbar placement="top" type="primary">
  <a slot="brand" href="{{ baseUrl }}/index.html" title="Home" class="navbar-brand">CS3281&2-{{ year }}/Students</a>
  <dropdown header="CS3281" class="nav-link">
    <li><a href="{{ baseUrl }}/index.html" class="dropdown-item">Students</a></li>
    <li><a href="{{ baseUrl }}/students/knowledge.html" class="dropdown-item">Knowledge</a></li>
    <li>
        <a href="https://nus-cs3281.github.io/{{ year }}-dashboard/?search=&sort=groupTitle&sortWithin=title&timeframe=commit&mergegroup=&groupSelect=groupByAuthors&breakdown=false" class="dropdown-item">Code Dashboard</a>
      </li>
  </dropdown>
  <dropdown header="CS3282" class="nav-link">
    <li><a href="{{ baseUrl }}/cs3282-index.html" class="dropdown-item">Students</a></li>
    <li><a href="{{ baseUrl }}/students/talksSchedule.html" class="dropdown-item">Lightning Talks</a></li>
  </dropdown>
  <li>
    <a href="{{ baseUrl }}/instructions.html" class="nav-link">Instructions</a>
  </li>
  <li>
    <a href="https://nus-cs3281.github.io/website/" class="nav-link">CS3281&2 Website <md>:glyphicon-share-alt:</md></a>
  </li>
  <li slot="right">
    <a href="https://github.com/nus-cs3281/{{ year }}" class="nav-link"><md>:fab-github:</md></a>
  </li>
</navbar>
</header>

<div id="flex-body">
  <div id="content-wrapper" class="fixed-header-padding">
    {{ content }}
  </div>
  <nav id="page-nav" class="fixed-header-padding">
    <div class="nav-component slim-scroll">
      <page-nav />
    </div>
  </nav>
</div>

<footer>
  <div class="text-center">

[**This site was generated using <img src="https://markbind.org/favicon.ico" width="25"/> {{MarkBind}}** on {{timestamp}}]<br>
    %%<small><small>favicon.ico of this site was made by <a href="https://www.flaticon.com/authors/smashicons" title="Smashicons">Smashicons</a> from <a href="https://www.flaticon.com/"     title="Flaticon">www.flaticon.com</a> is licensed by <a href="http://creativecommons.org/licenses/by/3.0/" title="Creative Commons BY 3.0" target="_blank">CC 3.0 BY</a></small></small>%%
  </div>
</footer>
