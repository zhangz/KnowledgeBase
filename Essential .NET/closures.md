



<!DOCTYPE html>
<html lang="en" class=" is-copy-enabled emoji-size-boost is-u2f-enabled">
  <head prefix="og: http://ogp.me/ns# fb: http://ogp.me/ns/fb# object: http://ogp.me/ns/object# article: http://ogp.me/ns/article# profile: http://ogp.me/ns/profile#">
    <meta charset='utf-8'>
    

    <link crossorigin="anonymous" href="https://assets-cdn.github.com/assets/frameworks-11a662d59757ef2ced3c65b6a175b2ebad3770c188a3b2cc0025b735fc78867c.css" integrity="sha256-EaZi1ZdX7yztPGW2oXWy6603cMGIo7LMACW3Nfx4hnw=" media="all" rel="stylesheet" />
    <link crossorigin="anonymous" href="https://assets-cdn.github.com/assets/github-ba7b20776e7af866228ad57120002664afac0392aecc45d2465d92a7f4c36a8d.css" integrity="sha256-unsgd256+GYiitVxIAAmZK+sA5KuzEXSRl2Sp/TDao0=" media="all" rel="stylesheet" />
    
    
    
    

    <link as="script" href="https://assets-cdn.github.com/assets/frameworks-655305f0453f424f8dab87dc1ae0e385e5982f72f7e2ce14eb1a9c229174cebe.js" rel="preload" />
    
    <link as="script" href="https://assets-cdn.github.com/assets/github-029e2067442a640842934c2eeeab2ba583d2d35dd1876e5d9c0ea092b5afe165.js" rel="preload" />

    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta http-equiv="Content-Language" content="en">
    <meta name="viewport" content="width=device-width">
    
    <title>aakinshin.net/closures.md at master · AndreyAkinshin/aakinshin.net</title>
    <link rel="search" type="application/opensearchdescription+xml" href="/opensearch.xml" title="GitHub">
    <link rel="fluid-icon" href="https://github.com/fluidicon.png" title="GitHub">
    <link rel="apple-touch-icon" href="/apple-touch-icon.png">
    <link rel="apple-touch-icon" sizes="57x57" href="/apple-touch-icon-57x57.png">
    <link rel="apple-touch-icon" sizes="60x60" href="/apple-touch-icon-60x60.png">
    <link rel="apple-touch-icon" sizes="72x72" href="/apple-touch-icon-72x72.png">
    <link rel="apple-touch-icon" sizes="76x76" href="/apple-touch-icon-76x76.png">
    <link rel="apple-touch-icon" sizes="114x114" href="/apple-touch-icon-114x114.png">
    <link rel="apple-touch-icon" sizes="120x120" href="/apple-touch-icon-120x120.png">
    <link rel="apple-touch-icon" sizes="144x144" href="/apple-touch-icon-144x144.png">
    <link rel="apple-touch-icon" sizes="152x152" href="/apple-touch-icon-152x152.png">
    <link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon-180x180.png">
    <meta property="fb:app_id" content="1401488693436528">

      <meta content="https://avatars0.githubusercontent.com/u/2259237?v=3&amp;s=400" name="twitter:image:src" /><meta content="@github" name="twitter:site" /><meta content="summary" name="twitter:card" /><meta content="AndreyAkinshin/aakinshin.net" name="twitter:title" /><meta content="aakinshin.net - My blog" name="twitter:description" />
      <meta content="https://avatars0.githubusercontent.com/u/2259237?v=3&amp;s=400" property="og:image" /><meta content="GitHub" property="og:site_name" /><meta content="object" property="og:type" /><meta content="AndreyAkinshin/aakinshin.net" property="og:title" /><meta content="https://github.com/AndreyAkinshin/aakinshin.net" property="og:url" /><meta content="aakinshin.net - My blog" property="og:description" />
      <meta name="browser-stats-url" content="https://api.github.com/_private/browser/stats">
    <meta name="browser-errors-url" content="https://api.github.com/_private/browser/errors">
    <link rel="assets" href="https://assets-cdn.github.com/">
    <link rel="web-socket" href="wss://live.github.com/_sockets/MTA4OTY1OjZhZDFmMzg1ZGIzN2NiN2MxNTlmMzBhNzliYWU4OTAzOjNjYTZjYjMxM2MwZTViN2QzYTcyOWExZTliMTMxYzg2ZjI5NzQxYWQzMjAxMGFjNTAxZGQ3ZDEzZTQxYjU3Nzc=--8f7137726ffd9dde67c68e69ce3cd20415ebb46c">
    <meta name="pjax-timeout" content="1000">
    <link rel="sudo-modal" href="/sessions/sudo_modal">
    <meta name="request-id" content="6B978BE4:6344:2B7E204:57B71B97" data-pjax-transient>

    <meta name="msapplication-TileImage" content="/windows-tile.png">
    <meta name="msapplication-TileColor" content="#ffffff">
    <meta name="selected-link" value="repo_source" data-pjax-transient>

    <meta name="google-site-verification" content="KT5gs8h0wvaagLKAVWq8bbeNwnZZK1r1XQysX3xurLU">
<meta name="google-site-verification" content="ZzhVyEFwb7w3e0-uOTltm8Jsck2F5StVihD0exw2fsA">
    <meta name="google-analytics" content="UA-3769691-2">

<meta content="collector.githubapp.com" name="octolytics-host" /><meta content="github" name="octolytics-app-id" /><meta content="6B978BE4:6344:2B7E204:57B71B97" name="octolytics-dimension-request_id" /><meta content="108965" name="octolytics-actor-id" /><meta content="zhangz" name="octolytics-actor-login" /><meta content="edcedb3432940fbf215fc577a2954e7bafaee17ee9b41497c240716a79ccf530" name="octolytics-actor-hash" />
<meta content="/&lt;user-name&gt;/&lt;repo-name&gt;/blob/show" data-pjax-transient="true" name="analytics-location" />



  <meta class="js-ga-set" name="dimension1" content="Logged In">



        <meta name="hostname" content="github.com">
    <meta name="user-login" content="zhangz">

        <meta name="expected-hostname" content="github.com">
      <meta name="js-proxy-site-detection-payload" content="M2FiZDUzNmY0MzU4Y2MwZDE0OTRmNDhjMjlkYTNlMWMyZTRmOTA5NzIzODUyNWE2YThmOTEzOWJkZmZlMjhkMXx7InJlbW90ZV9hZGRyZXNzIjoiMTA3LjE1MS4xMzkuMjI4IiwicmVxdWVzdF9pZCI6IjZCOTc4QkU0OjYzNDQ6MkI3RTIwNDo1N0I3MUI5NyIsInRpbWVzdGFtcCI6MTQ3MTYxNzk0OH0=">


      <link rel="mask-icon" href="https://assets-cdn.github.com/pinned-octocat.svg" color="#4078c0">
      <link rel="icon" type="image/x-icon" href="https://assets-cdn.github.com/favicon.ico">

    <meta name="html-safe-nonce" content="a43f3a7fd3b9b87e5b00ad3ff4e6bddb9cb24e2e">
    <meta content="7beea391c45528ad280696193254fe1c92c60bed" name="form-nonce" />

    <meta http-equiv="x-pjax-version" content="5920939eb2b6923c579ca040304db38d">
    

      
  <meta name="description" content="aakinshin.net - My blog">
  <meta name="go-import" content="github.com/AndreyAkinshin/aakinshin.net git https://github.com/AndreyAkinshin/aakinshin.net.git">

  <meta content="2259237" name="octolytics-dimension-user_id" /><meta content="AndreyAkinshin" name="octolytics-dimension-user_login" /><meta content="30122754" name="octolytics-dimension-repository_id" /><meta content="AndreyAkinshin/aakinshin.net" name="octolytics-dimension-repository_nwo" /><meta content="true" name="octolytics-dimension-repository_public" /><meta content="false" name="octolytics-dimension-repository_is_fork" /><meta content="30122754" name="octolytics-dimension-repository_network_root_id" /><meta content="AndreyAkinshin/aakinshin.net" name="octolytics-dimension-repository_network_root_nwo" />
  <link href="https://github.com/AndreyAkinshin/aakinshin.net/commits/master.atom" rel="alternate" title="Recent Commits to aakinshin.net:master" type="application/atom+xml">


      <link rel="canonical" href="https://github.com/AndreyAkinshin/aakinshin.net/blob/master/en/_posts/dotnet/closures.md" data-pjax-transient>
  </head>


  <body class="logged-in  env-production macintosh vis-public page-blob">
    <div id="js-pjax-loader-bar" class="pjax-loader-bar"><div class="progress"></div></div>
    <a href="#start-of-content" tabindex="1" class="accessibility-aid js-skip-to-content">Skip to content</a>

    
    
    



        <div class="header header-logged-in true" role="banner">
  <div class="container clearfix">

    <a class="header-logo-invertocat" href="https://github.com/" data-hotkey="g d" aria-label="Homepage" data-ga-click="Header, go to dashboard, icon:logo">
  <svg aria-hidden="true" class="octicon octicon-mark-github" height="28" version="1.1" viewBox="0 0 16 16" width="28"><path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0 0 16 8c0-4.42-3.58-8-8-8z"></path></svg>
</a>


        <div class="header-search scoped-search site-scoped-search js-site-search" role="search">
  <!-- </textarea> --><!-- '"` --><form accept-charset="UTF-8" action="/AndreyAkinshin/aakinshin.net/search" class="js-site-search-form" data-scoped-search-url="/AndreyAkinshin/aakinshin.net/search" data-unscoped-search-url="/search" method="get"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /></div>
    <label class="form-control header-search-wrapper js-chromeless-input-container">
      <div class="header-search-scope">This repository</div>
      <input type="text"
        class="form-control header-search-input js-site-search-focus js-site-search-field is-clearable"
        data-hotkey="s"
        name="q"
        placeholder="Search"
        aria-label="Search this repository"
        data-unscoped-placeholder="Search GitHub"
        data-scoped-placeholder="Search"
        autocapitalize="off">
    </label>
</form></div>


      <ul class="header-nav left" role="navigation">
        <li class="header-nav-item">
          <a href="/pulls" class="js-selected-navigation-item header-nav-link" data-ga-click="Header, click, Nav menu - item:pulls context:user" data-hotkey="g p" data-selected-links="/pulls /pulls/assigned /pulls/mentioned /pulls">
            Pull requests
</a>        </li>
        <li class="header-nav-item">
          <a href="/issues" class="js-selected-navigation-item header-nav-link" data-ga-click="Header, click, Nav menu - item:issues context:user" data-hotkey="g i" data-selected-links="/issues /issues/assigned /issues/mentioned /issues">
            Issues
</a>        </li>
          <li class="header-nav-item">
            <a class="header-nav-link" href="https://gist.github.com/" data-ga-click="Header, go to gist, text:gist">Gist</a>
          </li>
      </ul>

    
<ul class="header-nav user-nav right" id="user-links">
  <li class="header-nav-item">
    
    <a href="/notifications" aria-label="You have no unread notifications" class="header-nav-link notification-indicator tooltipped tooltipped-s js-socket-channel js-notification-indicator" data-channel="tenant:1:notification-changed:108965" data-ga-click="Header, go to notifications, icon:read" data-hotkey="g n">
        <span class="mail-status "></span>
        <svg aria-hidden="true" class="octicon octicon-bell" height="16" version="1.1" viewBox="0 0 14 16" width="14"><path d="M14 12v1H0v-1l.73-.58c.77-.77.81-2.55 1.19-4.42C2.69 3.23 6 2 6 2c0-.55.45-1 1-1s1 .45 1 1c0 0 3.39 1.23 4.16 5 .38 1.88.42 3.66 1.19 4.42l.66.58H14zm-7 4c1.11 0 2-.89 2-2H5c0 1.11.89 2 2 2z"></path></svg>
</a>
  </li>

  <li class="header-nav-item dropdown js-menu-container">
    <a class="header-nav-link tooltipped tooltipped-s js-menu-target" href="/new"
       aria-label="Create new…"
       data-ga-click="Header, create new, icon:add">
      <svg aria-hidden="true" class="octicon octicon-plus left" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M12 9H7v5H5V9H0V7h5V2h2v5h5z"></path></svg>
      <span class="dropdown-caret"></span>
    </a>

    <div class="dropdown-menu-content js-menu-content">
      <ul class="dropdown-menu dropdown-menu-sw">
        
<a class="dropdown-item" href="/new" data-ga-click="Header, create new repository">
  New repository
</a>

  <a class="dropdown-item" href="/new/import" data-ga-click="Header, import a repository">
    Import repository
  </a>


  <a class="dropdown-item" href="/organizations/new" data-ga-click="Header, create new organization">
    New organization
  </a>




      </ul>
    </div>
  </li>

  <li class="header-nav-item dropdown js-menu-container">
    <a class="header-nav-link name tooltipped tooltipped-sw js-menu-target" href="/zhangz"
       aria-label="View profile and more"
       data-ga-click="Header, show menu, icon:avatar">
      <img alt="@zhangz" class="avatar" height="20" src="https://avatars3.githubusercontent.com/u/108965?v=3&amp;s=40" width="20" />
      <span class="dropdown-caret"></span>
    </a>

    <div class="dropdown-menu-content js-menu-content">
      <div class="dropdown-menu dropdown-menu-sw">
        <div class="dropdown-header header-nav-current-user css-truncate">
          Signed in as <strong class="css-truncate-target">zhangz</strong>
        </div>

        <div class="dropdown-divider"></div>

        <a class="dropdown-item" href="/zhangz" data-ga-click="Header, go to profile, text:your profile">
          Your profile
        </a>
        <a class="dropdown-item" href="/stars" data-ga-click="Header, go to starred repos, text:your stars">
          Your stars
        </a>
        <a class="dropdown-item" href="/explore" data-ga-click="Header, go to explore, text:explore">
          Explore
        </a>
          <a class="dropdown-item" href="/integrations" data-ga-click="Header, go to integrations, text:integrations">
            Integrations
          </a>
        <a class="dropdown-item" href="https://help.github.com" data-ga-click="Header, go to help, text:help">
          Help
        </a>


        <div class="dropdown-divider"></div>

        <a class="dropdown-item" href="/settings/profile" data-ga-click="Header, go to settings, icon:settings">
          Settings
        </a>

        <!-- </textarea> --><!-- '"` --><form accept-charset="UTF-8" action="/logout" class="logout-form" data-form-nonce="7beea391c45528ad280696193254fe1c92c60bed" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="MONOhrRSrulZI3zSbBgWAJzkuT+xu3DNQlRvPBVT0ZDluqVsLT3y6slMqWhwSN0psdYijGonKzGRSyDR3VFQlQ==" /></div>
          <button class="dropdown-item dropdown-signout" data-ga-click="Header, sign out, icon:logout">
            Sign out
          </button>
</form>      </div>
    </div>
  </li>
</ul>


    
  </div>
</div>


      


    <div id="start-of-content" class="accessibility-aid"></div>

      <div id="js-flash-container">
</div>


    <div role="main">
        <div itemscope itemtype="http://schema.org/SoftwareSourceCode">
    <div id="js-repo-pjax-container" data-pjax-container>
      
<div class="pagehead repohead instapaper_ignore readability-menu experiment-repo-nav">
  <div class="container repohead-details-container">

    

<ul class="pagehead-actions">

  <li>
        <!-- </textarea> --><!-- '"` --><form accept-charset="UTF-8" action="/notifications/subscribe" class="js-social-container" data-autosubmit="true" data-form-nonce="7beea391c45528ad280696193254fe1c92c60bed" data-remote="true" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="ReEnAS+r0E6LKW5uQTrNGAqQQ+vA7unEFBk+/5O62hmHYMkuTxLp+xJHpuF92CmMHRCCfnDCbUDJpiVkcVioEQ==" /></div>      <input class="form-control" id="repository_id" name="repository_id" type="hidden" value="30122754" />

        <div class="select-menu js-menu-container js-select-menu">
          <a href="/AndreyAkinshin/aakinshin.net/subscription"
            class="btn btn-sm btn-with-count select-menu-button js-menu-target" role="button" tabindex="0" aria-haspopup="true"
            data-ga-click="Repository, click Watch settings, action:blob#show">
            <span class="js-select-button">
              <svg aria-hidden="true" class="octicon octicon-eye" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M8.06 2C3 2 0 8 0 8s3 6 8.06 6C13 14 16 8 16 8s-3-6-7.94-6zM8 12c-2.2 0-4-1.78-4-4 0-2.2 1.8-4 4-4 2.22 0 4 1.8 4 4 0 2.22-1.78 4-4 4zm2-4c0 1.11-.89 2-2 2-1.11 0-2-.89-2-2 0-1.11.89-2 2-2 1.11 0 2 .89 2 2z"></path></svg>
              Watch
            </span>
          </a>
          <a class="social-count js-social-count" href="/AndreyAkinshin/aakinshin.net/watchers">
            3
          </a>

        <div class="select-menu-modal-holder">
          <div class="select-menu-modal subscription-menu-modal js-menu-content" aria-hidden="true">
            <div class="select-menu-header js-navigation-enable" tabindex="-1">
              <svg aria-label="Close" class="octicon octicon-x js-menu-close" height="16" role="img" version="1.1" viewBox="0 0 12 16" width="12"><path d="M7.48 8l3.75 3.75-1.48 1.48L6 9.48l-3.75 3.75-1.48-1.48L4.52 8 .77 4.25l1.48-1.48L6 6.52l3.75-3.75 1.48 1.48z"></path></svg>
              <span class="select-menu-title">Notifications</span>
            </div>

              <div class="select-menu-list js-navigation-container" role="menu">

                <div class="select-menu-item js-navigation-item selected" role="menuitem" tabindex="0">
                  <svg aria-hidden="true" class="octicon octicon-check select-menu-item-icon" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M12 5l-8 8-4-4 1.5-1.5L4 10l6.5-6.5z"></path></svg>
                  <div class="select-menu-item-text">
                    <input checked="checked" id="do_included" name="do" type="radio" value="included" />
                    <span class="select-menu-item-heading">Not watching</span>
                    <span class="description">Be notified when participating or @mentioned.</span>
                    <span class="js-select-button-text hidden-select-button-text">
                      <svg aria-hidden="true" class="octicon octicon-eye" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M8.06 2C3 2 0 8 0 8s3 6 8.06 6C13 14 16 8 16 8s-3-6-7.94-6zM8 12c-2.2 0-4-1.78-4-4 0-2.2 1.8-4 4-4 2.22 0 4 1.8 4 4 0 2.22-1.78 4-4 4zm2-4c0 1.11-.89 2-2 2-1.11 0-2-.89-2-2 0-1.11.89-2 2-2 1.11 0 2 .89 2 2z"></path></svg>
                      Watch
                    </span>
                  </div>
                </div>

                <div class="select-menu-item js-navigation-item " role="menuitem" tabindex="0">
                  <svg aria-hidden="true" class="octicon octicon-check select-menu-item-icon" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M12 5l-8 8-4-4 1.5-1.5L4 10l6.5-6.5z"></path></svg>
                  <div class="select-menu-item-text">
                    <input id="do_subscribed" name="do" type="radio" value="subscribed" />
                    <span class="select-menu-item-heading">Watching</span>
                    <span class="description">Be notified of all conversations.</span>
                    <span class="js-select-button-text hidden-select-button-text">
                      <svg aria-hidden="true" class="octicon octicon-eye" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M8.06 2C3 2 0 8 0 8s3 6 8.06 6C13 14 16 8 16 8s-3-6-7.94-6zM8 12c-2.2 0-4-1.78-4-4 0-2.2 1.8-4 4-4 2.22 0 4 1.8 4 4 0 2.22-1.78 4-4 4zm2-4c0 1.11-.89 2-2 2-1.11 0-2-.89-2-2 0-1.11.89-2 2-2 1.11 0 2 .89 2 2z"></path></svg>
                      Unwatch
                    </span>
                  </div>
                </div>

                <div class="select-menu-item js-navigation-item " role="menuitem" tabindex="0">
                  <svg aria-hidden="true" class="octicon octicon-check select-menu-item-icon" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M12 5l-8 8-4-4 1.5-1.5L4 10l6.5-6.5z"></path></svg>
                  <div class="select-menu-item-text">
                    <input id="do_ignore" name="do" type="radio" value="ignore" />
                    <span class="select-menu-item-heading">Ignoring</span>
                    <span class="description">Never be notified.</span>
                    <span class="js-select-button-text hidden-select-button-text">
                      <svg aria-hidden="true" class="octicon octicon-mute" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M8 2.81v10.38c0 .67-.81 1-1.28.53L3 10H1c-.55 0-1-.45-1-1V7c0-.55.45-1 1-1h2l3.72-3.72C7.19 1.81 8 2.14 8 2.81zm7.53 3.22l-1.06-1.06-1.97 1.97-1.97-1.97-1.06 1.06L11.44 8 9.47 9.97l1.06 1.06 1.97-1.97 1.97 1.97 1.06-1.06L13.56 8l1.97-1.97z"></path></svg>
                      Stop ignoring
                    </span>
                  </div>
                </div>

              </div>

            </div>
          </div>
        </div>
</form>
  </li>

  <li>
    
  <div class="js-toggler-container js-social-container starring-container ">

    <!-- </textarea> --><!-- '"` --><form accept-charset="UTF-8" action="/AndreyAkinshin/aakinshin.net/unstar" class="starred" data-form-nonce="7beea391c45528ad280696193254fe1c92c60bed" data-remote="true" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="enQBEQFucVmgGy4K5LFrOdoVbwAAhUxiueb6InOIjdVwP2e+d3ntAGgDMujFcNThAXT8sasWZfuSlbkvXz2CGg==" /></div>
      <button
        class="btn btn-sm btn-with-count js-toggler-target"
        aria-label="Unstar this repository" title="Unstar AndreyAkinshin/aakinshin.net"
        data-ga-click="Repository, click unstar button, action:blob#show; text:Unstar">
        <svg aria-hidden="true" class="octicon octicon-star" height="16" version="1.1" viewBox="0 0 14 16" width="14"><path d="M14 6l-4.9-.64L7 1 4.9 5.36 0 6l3.6 3.26L2.67 14 7 11.67 11.33 14l-.93-4.74z"></path></svg>
        Unstar
      </button>
        <a class="social-count js-social-count" href="/AndreyAkinshin/aakinshin.net/stargazers">
          11
        </a>
</form>
    <!-- </textarea> --><!-- '"` --><form accept-charset="UTF-8" action="/AndreyAkinshin/aakinshin.net/star" class="unstarred" data-form-nonce="7beea391c45528ad280696193254fe1c92c60bed" data-remote="true" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="O0238e46XKfMUVYuxZE053UnpaB5X0M1wKQ4WuH+W29R6hp7Q3YOlzQYDHzix/7lijU+ocwkV3RHGjuQJ8AHdg==" /></div>
      <button
        class="btn btn-sm btn-with-count js-toggler-target"
        aria-label="Star this repository" title="Star AndreyAkinshin/aakinshin.net"
        data-ga-click="Repository, click star button, action:blob#show; text:Star">
        <svg aria-hidden="true" class="octicon octicon-star" height="16" version="1.1" viewBox="0 0 14 16" width="14"><path d="M14 6l-4.9-.64L7 1 4.9 5.36 0 6l3.6 3.26L2.67 14 7 11.67 11.33 14l-.93-4.74z"></path></svg>
        Star
      </button>
        <a class="social-count js-social-count" href="/AndreyAkinshin/aakinshin.net/stargazers">
          11
        </a>
</form>  </div>

  </li>

  <li>
          <!-- </textarea> --><!-- '"` --><form accept-charset="UTF-8" action="/AndreyAkinshin/aakinshin.net/fork" class="btn-with-count" data-form-nonce="7beea391c45528ad280696193254fe1c92c60bed" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="vFzdg8ely7LmncS1oHeRUVPQspI0Go11mpyv0LeV3Dtx0irhVtL2dz8Z+JdAlQx1rv0iYF2UBGromCVGb07+Yg==" /></div>
            <button
                type="submit"
                class="btn btn-sm btn-with-count"
                data-ga-click="Repository, show fork modal, action:blob#show; text:Fork"
                title="Fork your own copy of AndreyAkinshin/aakinshin.net to your account"
                aria-label="Fork your own copy of AndreyAkinshin/aakinshin.net to your account">
              <svg aria-hidden="true" class="octicon octicon-repo-forked" height="16" version="1.1" viewBox="0 0 10 16" width="10"><path d="M8 1a1.993 1.993 0 0 0-1 3.72V6L5 8 3 6V4.72A1.993 1.993 0 0 0 2 1a1.993 1.993 0 0 0-1 3.72V6.5l3 3v1.78A1.993 1.993 0 0 0 5 15a1.993 1.993 0 0 0 1-3.72V9.5l3-3V4.72A1.993 1.993 0 0 0 8 1zM2 4.2C1.34 4.2.8 3.65.8 3c0-.65.55-1.2 1.2-1.2.65 0 1.2.55 1.2 1.2 0 .65-.55 1.2-1.2 1.2zm3 10c-.66 0-1.2-.55-1.2-1.2 0-.65.55-1.2 1.2-1.2.65 0 1.2.55 1.2 1.2 0 .65-.55 1.2-1.2 1.2zm3-10c-.66 0-1.2-.55-1.2-1.2 0-.65.55-1.2 1.2-1.2.65 0 1.2.55 1.2 1.2 0 .65-.55 1.2-1.2 1.2z"></path></svg>
              Fork
            </button>
</form>
    <a href="/AndreyAkinshin/aakinshin.net/network" class="social-count">
      7
    </a>
  </li>
</ul>

    <h1 class="public ">
  <svg aria-hidden="true" class="octicon octicon-repo" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M4 9H3V8h1v1zm0-3H3v1h1V6zm0-2H3v1h1V4zm0-2H3v1h1V2zm8-1v12c0 .55-.45 1-1 1H6v2l-1.5-1.5L3 16v-2H1c-.55 0-1-.45-1-1V1c0-.55.45-1 1-1h10c.55 0 1 .45 1 1zm-1 10H1v2h2v-1h3v1h5v-2zm0-10H2v9h9V1z"></path></svg>
  <span class="author" itemprop="author"><a href="/AndreyAkinshin" class="url fn" rel="author">AndreyAkinshin</a></span><!--
--><span class="path-divider">/</span><!--
--><strong itemprop="name"><a href="/AndreyAkinshin/aakinshin.net" data-pjax="#js-repo-pjax-container">aakinshin.net</a></strong>

</h1>

  </div>
  <div class="container">
    
<nav class="reponav js-repo-nav js-sidenav-container-pjax"
     itemscope
     itemtype="http://schema.org/BreadcrumbList"
     role="navigation"
     data-pjax="#js-repo-pjax-container">

  <span itemscope itemtype="http://schema.org/ListItem" itemprop="itemListElement">
    <a href="/AndreyAkinshin/aakinshin.net" aria-selected="true" class="js-selected-navigation-item selected reponav-item" data-hotkey="g c" data-selected-links="repo_source repo_downloads repo_commits repo_releases repo_tags repo_branches /AndreyAkinshin/aakinshin.net" itemprop="url">
      <svg aria-hidden="true" class="octicon octicon-code" height="16" version="1.1" viewBox="0 0 14 16" width="14"><path d="M9.5 3L8 4.5 11.5 8 8 11.5 9.5 13 14 8 9.5 3zm-5 0L0 8l4.5 5L6 11.5 2.5 8 6 4.5 4.5 3z"></path></svg>
      <span itemprop="name">Code</span>
      <meta itemprop="position" content="1">
</a>  </span>


  <span itemscope itemtype="http://schema.org/ListItem" itemprop="itemListElement">
    <a href="/AndreyAkinshin/aakinshin.net/pulls" class="js-selected-navigation-item reponav-item" data-hotkey="g p" data-selected-links="repo_pulls /AndreyAkinshin/aakinshin.net/pulls" itemprop="url">
      <svg aria-hidden="true" class="octicon octicon-git-pull-request" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M11 11.28V5c-.03-.78-.34-1.47-.94-2.06C9.46 2.35 8.78 2.03 8 2H7V0L4 3l3 3V4h1c.27.02.48.11.69.31.21.2.3.42.31.69v6.28A1.993 1.993 0 0 0 10 15a1.993 1.993 0 0 0 1-3.72zm-1 2.92c-.66 0-1.2-.55-1.2-1.2 0-.65.55-1.2 1.2-1.2.65 0 1.2.55 1.2 1.2 0 .65-.55 1.2-1.2 1.2zM4 3c0-1.11-.89-2-2-2a1.993 1.993 0 0 0-1 3.72v6.56A1.993 1.993 0 0 0 2 15a1.993 1.993 0 0 0 1-3.72V4.72c.59-.34 1-.98 1-1.72zm-.8 10c0 .66-.55 1.2-1.2 1.2-.65 0-1.2-.55-1.2-1.2 0-.65.55-1.2 1.2-1.2.65 0 1.2.55 1.2 1.2zM2 4.2C1.34 4.2.8 3.65.8 3c0-.65.55-1.2 1.2-1.2.65 0 1.2.55 1.2 1.2 0 .65-.55 1.2-1.2 1.2z"></path></svg>
      <span itemprop="name">Pull requests</span>
      <span class="counter">0</span>
      <meta itemprop="position" content="3">
</a>  </span>




  <a href="/AndreyAkinshin/aakinshin.net/pulse" class="js-selected-navigation-item reponav-item" data-selected-links="pulse /AndreyAkinshin/aakinshin.net/pulse">
    <svg aria-hidden="true" class="octicon octicon-pulse" height="16" version="1.1" viewBox="0 0 14 16" width="14"><path d="M11.5 8L8.8 5.4 6.6 8.5 5.5 1.6 2.38 8H0v2h3.6l.9-1.8.9 5.4L9 8.5l1.6 1.5H14V8z"></path></svg>
    Pulse
</a>
  <a href="/AndreyAkinshin/aakinshin.net/graphs" class="js-selected-navigation-item reponav-item" data-selected-links="repo_graphs repo_contributors /AndreyAkinshin/aakinshin.net/graphs">
    <svg aria-hidden="true" class="octicon octicon-graph" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M16 14v1H0V0h1v14h15zM5 13H3V8h2v5zm4 0H7V3h2v10zm4 0h-2V6h2v7z"></path></svg>
    Graphs
</a>

</nav>

  </div>
</div>

<div class="container new-discussion-timeline experiment-repo-nav">
  <div class="repository-content">

    

<a href="/AndreyAkinshin/aakinshin.net/blob/ae64080a8701a102eec192c48f141918091f545a/en/_posts/dotnet/closures.md" class="hidden js-permalink-shortcut" data-hotkey="y">Permalink</a>

<!-- blob contrib key: blob_contributors:v21:c22fa1b387c56105927b6796082b3101 -->

<div class="file-navigation js-zeroclipboard-container">
  
<div class="select-menu branch-select-menu js-menu-container js-select-menu left">
  <button class="btn btn-sm select-menu-button js-menu-target css-truncate" data-hotkey="w"
    
    type="button" aria-label="Switch branches or tags" tabindex="0" aria-haspopup="true">
    <i>Branch:</i>
    <span class="js-select-button css-truncate-target">master</span>
  </button>

  <div class="select-menu-modal-holder js-menu-content js-navigation-container" data-pjax aria-hidden="true">

    <div class="select-menu-modal">
      <div class="select-menu-header">
        <svg aria-label="Close" class="octicon octicon-x js-menu-close" height="16" role="img" version="1.1" viewBox="0 0 12 16" width="12"><path d="M7.48 8l3.75 3.75-1.48 1.48L6 9.48l-3.75 3.75-1.48-1.48L4.52 8 .77 4.25l1.48-1.48L6 6.52l3.75-3.75 1.48 1.48z"></path></svg>
        <span class="select-menu-title">Switch branches/tags</span>
      </div>

      <div class="select-menu-filters">
        <div class="select-menu-text-filter">
          <input type="text" aria-label="Filter branches/tags" id="context-commitish-filter-field" class="form-control js-filterable-field js-navigation-enable" placeholder="Filter branches/tags">
        </div>
        <div class="select-menu-tabs">
          <ul>
            <li class="select-menu-tab">
              <a href="#" data-tab-filter="branches" data-filter-placeholder="Filter branches/tags" class="js-select-menu-tab" role="tab">Branches</a>
            </li>
            <li class="select-menu-tab">
              <a href="#" data-tab-filter="tags" data-filter-placeholder="Find a tag…" class="js-select-menu-tab" role="tab">Tags</a>
            </li>
          </ul>
        </div>
      </div>

      <div class="select-menu-list select-menu-tab-bucket js-select-menu-tab-bucket" data-tab-filter="branches" role="menu">

        <div data-filterable-for="context-commitish-filter-field" data-filterable-type="substring">


            <a class="select-menu-item js-navigation-item js-navigation-open "
               href="/AndreyAkinshin/aakinshin.net/blob/drafts/en/_posts/dotnet/closures.md"
               data-name="drafts"
               data-skip-pjax="true"
               rel="nofollow">
              <svg aria-hidden="true" class="octicon octicon-check select-menu-item-icon" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M12 5l-8 8-4-4 1.5-1.5L4 10l6.5-6.5z"></path></svg>
              <span class="select-menu-item-text css-truncate-target js-select-menu-filter-text">
                drafts
              </span>
            </a>
            <a class="select-menu-item js-navigation-item js-navigation-open "
               href="/AndreyAkinshin/aakinshin.net/blob/gh-pages/en/_posts/dotnet/closures.md"
               data-name="gh-pages"
               data-skip-pjax="true"
               rel="nofollow">
              <svg aria-hidden="true" class="octicon octicon-check select-menu-item-icon" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M12 5l-8 8-4-4 1.5-1.5L4 10l6.5-6.5z"></path></svg>
              <span class="select-menu-item-text css-truncate-target js-select-menu-filter-text">
                gh-pages
              </span>
            </a>
            <a class="select-menu-item js-navigation-item js-navigation-open selected"
               href="/AndreyAkinshin/aakinshin.net/blob/master/en/_posts/dotnet/closures.md"
               data-name="master"
               data-skip-pjax="true"
               rel="nofollow">
              <svg aria-hidden="true" class="octicon octicon-check select-menu-item-icon" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M12 5l-8 8-4-4 1.5-1.5L4 10l6.5-6.5z"></path></svg>
              <span class="select-menu-item-text css-truncate-target js-select-menu-filter-text">
                master
              </span>
            </a>
        </div>

          <div class="select-menu-no-results">Nothing to show</div>
      </div>

      <div class="select-menu-list select-menu-tab-bucket js-select-menu-tab-bucket" data-tab-filter="tags">
        <div data-filterable-for="context-commitish-filter-field" data-filterable-type="substring">


        </div>

        <div class="select-menu-no-results">Nothing to show</div>
      </div>

    </div>
  </div>
</div>

  <div class="btn-group right">
    <a href="/AndreyAkinshin/aakinshin.net/find/master"
          class="js-pjax-capture-input btn btn-sm"
          data-pjax
          data-hotkey="t">
      Find file
    </a>
    <button aria-label="Copy file path to clipboard" class="js-zeroclipboard btn btn-sm zeroclipboard-button tooltipped tooltipped-s" data-copied-hint="Copied!" type="button">Copy path</button>
  </div>
  <div class="breadcrumb js-zeroclipboard-target">
    <span class="repo-root js-repo-root"><span class="js-path-segment"><a href="/AndreyAkinshin/aakinshin.net"><span>aakinshin.net</span></a></span></span><span class="separator">/</span><span class="js-path-segment"><a href="/AndreyAkinshin/aakinshin.net/tree/master/en"><span>en</span></a></span><span class="separator">/</span><span class="js-path-segment"><a href="/AndreyAkinshin/aakinshin.net/tree/master/en/_posts"><span>_posts</span></a></span><span class="separator">/</span><span class="js-path-segment"><a href="/AndreyAkinshin/aakinshin.net/tree/master/en/_posts/dotnet"><span>dotnet</span></a></span><span class="separator">/</span><strong class="final-path">closures.md</strong>
  </div>
</div>


  <div class="commit-tease">
      <span class="right">
        <a class="commit-tease-sha" href="/AndreyAkinshin/aakinshin.net/commit/0e2f10bad7a8201d6961376cd793f93beb5e6843" data-pjax>
          0e2f10b
        </a>
        <relative-time datetime="2016-01-06T09:56:04Z">Jan 6, 2016</relative-time>
      </span>
      <div>
        <img alt="@AndreyAkinshin" class="avatar" height="20" src="https://avatars3.githubusercontent.com/u/2259237?v=3&amp;s=40" width="20" />
        <a href="/AndreyAkinshin" class="user-mention" rel="author">AndreyAkinshin</a>
          <a href="/AndreyAkinshin/aakinshin.net/commit/0e2f10bad7a8201d6961376cd793f93beb5e6843" class="message" data-pjax="true" title="Big refactoring">Big refactoring</a>
      </div>

    <div class="commit-tease-contributors">
      <button type="button" class="btn-link muted-link contributors-toggle" data-facebox="#blob_contributors_box">
        <strong>1</strong>
         contributor
      </button>
      
    </div>

    <div id="blob_contributors_box" style="display:none">
      <h2 class="facebox-header" data-facebox-id="facebox-header">Users who have contributed to this file</h2>
      <ul class="facebox-user-list" data-facebox-id="facebox-description">
          <li class="facebox-user-list-item">
            <img alt="@AndreyAkinshin" height="24" src="https://avatars1.githubusercontent.com/u/2259237?v=3&amp;s=48" width="24" />
            <a href="/AndreyAkinshin">AndreyAkinshin</a>
          </li>
      </ul>
    </div>
  </div>

<div class="file">
  <div class="file-header">
  <div class="file-actions">

    <div class="btn-group">
      <a href="/AndreyAkinshin/aakinshin.net/raw/master/en/_posts/dotnet/closures.md" class="btn btn-sm " id="raw-url">Raw</a>
        <a href="/AndreyAkinshin/aakinshin.net/blame/master/en/_posts/dotnet/closures.md" class="btn btn-sm js-update-url-with-hash">Blame</a>
      <a href="/AndreyAkinshin/aakinshin.net/commits/master/en/_posts/dotnet/closures.md" class="btn btn-sm " rel="nofollow">History</a>
    </div>

        <a class="btn-octicon tooltipped tooltipped-nw"
           href="github-mac://openRepo/https://github.com/AndreyAkinshin/aakinshin.net?branch=master&amp;filepath=en%2F_posts%2Fdotnet%2Fclosures.md"
           aria-label="Open this file in GitHub Desktop"
           data-ga-click="Repository, open with desktop, type:mac">
            <svg aria-hidden="true" class="octicon octicon-device-desktop" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M15 2H1c-.55 0-1 .45-1 1v9c0 .55.45 1 1 1h5.34c-.25.61-.86 1.39-2.34 2h8c-1.48-.61-2.09-1.39-2.34-2H15c.55 0 1-.45 1-1V3c0-.55-.45-1-1-1zm0 9H1V3h14v8z"></path></svg>
        </a>

        <!-- </textarea> --><!-- '"` --><form accept-charset="UTF-8" action="/AndreyAkinshin/aakinshin.net/edit/master/en/_posts/dotnet/closures.md" class="inline-form js-update-url-with-hash" data-form-nonce="7beea391c45528ad280696193254fe1c92c60bed" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="JRzTckFp6O5khIZoJrc7HNzMeqgyN6aOiBiACYc3w7AMUDATm+m6U0Qy4X7XAFnuvXQCGoAuCci6UStrOWWrNA==" /></div>
          <button class="btn-octicon tooltipped tooltipped-nw" type="submit"
            aria-label="Fork this project and edit the file" data-hotkey="e" data-disable-with>
            <svg aria-hidden="true" class="octicon octicon-pencil" height="16" version="1.1" viewBox="0 0 14 16" width="14"><path d="M0 12v3h3l8-8-3-3-8 8zm3 2H1v-2h1v1h1v1zm10.3-9.3L12 6 9 3l1.3-1.3a.996.996 0 0 1 1.41 0l1.59 1.59c.39.39.39 1.02 0 1.41z"></path></svg>
          </button>
</form>        <!-- </textarea> --><!-- '"` --><form accept-charset="UTF-8" action="/AndreyAkinshin/aakinshin.net/delete/master/en/_posts/dotnet/closures.md" class="inline-form" data-form-nonce="7beea391c45528ad280696193254fe1c92c60bed" method="post"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /><input name="authenticity_token" type="hidden" value="Wsr9xqEU11GnyGoMuopYrxUwXQ+2P1w7pI7aesJaQ//PVVwqWI108qOnjs24jsHUUXGY5TXexEI89QNQJn5OLA==" /></div>
          <button class="btn-octicon btn-octicon-danger tooltipped tooltipped-nw" type="submit"
            aria-label="Fork this project and delete the file" data-disable-with>
            <svg aria-hidden="true" class="octicon octicon-trashcan" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M11 2H9c0-.55-.45-1-1-1H5c-.55 0-1 .45-1 1H2c-.55 0-1 .45-1 1v1c0 .55.45 1 1 1v9c0 .55.45 1 1 1h7c.55 0 1-.45 1-1V5c.55 0 1-.45 1-1V3c0-.55-.45-1-1-1zm-1 12H3V5h1v8h1V5h1v8h1V5h1v8h1V5h1v9zm1-10H2V3h9v1z"></path></svg>
          </button>
</form>  </div>

  <div class="file-info">
      265 lines (217 sloc)
      <span class="file-info-divider"></span>
    7.44 KB
  </div>
</div>

  
  <div id="readme" class="readme blob instapaper_body">
    <article class="markdown-body entry-content" itemprop="text"><table data-table-type="yaml-metadata">
  <thead>
  <tr>
  <th>layout</th>

  <th>title</th>

  <th>date</th>

  <th>categories</th>

  <th>tags</th>

  <th>modified_time</th>

  <th>blogger_id</th>

  <th>blogger_orig_url</th>
  </tr>
  </thead>
  <tbody>
  <tr>
  <td><div>post</div></td>

  <td><div>Unobviousness in use of C# closures</div></td>

  <td><div>2013-08-07T16:35:00.000+07:00</div></td>

  <td><div><table>
  <tbody>
  <tr>
  <td><div>dotnet</div></td>
  </tr>
  </tbody>
</table></div></td>

  <td><div><table>
  <tbody>
  <tr>
  <td><div>Lambda</div></td>

  <td><div>Closures</div></td>

  <td><div>C#</div></td>

  <td><div>.NET</div></td>
  </tr>
  </tbody>
</table></div></td>

  <td><div>2015-01-24T05:42:39.845+06:00</div></td>

  <td><div>tag:blogger.com,1999:blog-8501021762411496121.post-555571649760142687</div></td>

  <td><div><a href="http://aakinshin.blogspot.com/2013/08/cs-closures.html">http://aakinshin.blogspot.com/2013/08/cs-closures.html</a></div></td>
  </tr>
  </tbody>
</table>

<p>C# gives us an ability to use closures. This is a powerful tool that allows anonymous methods and lambda-functions to capture unbound variables in their lexical scope. And many programmers in .NET world like using closures very much, but only few of them understand how they really work. Let’s start with a simple sample:</p>

<div class="highlight highlight-source-cs"><pre><span class="pl-k">public</span> <span class="pl-k">void</span> Run()
{
  <span class="pl-k">int</span> e = <span class="pl-c1">1</span>;
  Foo(x =&gt; x + e);
}</pre></div>

<p>Nothing complicated happens here: we just captured a local variable <code>e</code> in its lambda that is passed to some <code>Foo</code> method. Let’s see how the compiler will expand such construction.*</p>

<div class="highlight highlight-source-cs"><pre><span class="pl-k">public</span> <span class="pl-k">void</span> Run()
{
  DisplayClass c = <span class="pl-k">new</span> DisplayClass();
  c.e = <span class="pl-c1">1</span>;  
  Foo(c.Action);
}
<span class="pl-k">private</span> <span class="pl-k">sealed</span> <span class="pl-k">class</span> <span class="pl-en">DisplayClass</span>
{
  <span class="pl-k">public</span> <span class="pl-k">int</span> e;
  <span class="pl-k">public</span> <span class="pl-k">int</span> <span class="pl-en">Action</span>(<span class="pl-k">int</span> <span class="pl-smi">x</span>)
  {
    <span class="pl-k">return</span> x + e;
  }
}</pre></div>



<p>As you see from the sample, an additional class containing the captured variable and the target method is created for our closure. This knowledge will help us understand how closures behave in different situations.</p>

<h3><a id="user-content-the-for-loop" class="anchor" href="#the-for-loop" aria-hidden="true"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>The for loop</h3>

<p>Probably, this is the most classic sample cited by everyone:</p>

<pre><code>public void Run()
{
  var actions = new List&lt;Action&gt;();
  for (int i = 0; i &lt; 3; i++)
    actions.Add(() =&gt; Console.WriteLine(i));
  foreach (var action in actions)
    action();
}
</code></pre>

<p>The sample contains a typical error. Newbie developers think that this code will output <code>"0 1 2"</code>, but in fact it will output <code>"3 3 3"</code>. Such strange behavior is easy to understand if you look on the expanded version of this method:</p>

<div class="highlight highlight-source-cs"><pre><span class="pl-k">public</span> <span class="pl-k">void</span> Run()
{
  <span class="pl-k">var</span> actions = <span class="pl-k">new</span> List&lt;Action&gt;();
  DisplayClass c = <span class="pl-k">new</span> DisplayClass();
  <span class="pl-k">for</span> (c.i = <span class="pl-c1">0</span>; c.i &lt; <span class="pl-c1">3</span>; c.i++)
    list.Add(c.Action);
  <span class="pl-k">foreach</span> (Action action <span class="pl-k">in</span> list)
    action();
}

<span class="pl-k">private</span> <span class="pl-k">sealed</span> <span class="pl-k">class</span> <span class="pl-en">DisplayClass</span>
{
  <span class="pl-k">public</span> <span class="pl-k">int</span> i;

  <span class="pl-k">public</span> <span class="pl-k">void</span> <span class="pl-en">Action</span>()
  {
    Console.WriteLine(i);
  }
}</pre></div>

<p>In this case they say that the variable is cycled by reference, not by value. Many programmers criticize this peculiarity of closures. They think it’s unclear though it’s quite logical for those who get a clear idea what’s inside the closures.</p>

<h3><a id="user-content-the-foreach-loop" class="anchor" href="#the-foreach-loop" aria-hidden="true"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>The foreach loop</h3>

<p>Let’s review a more interesting sample:</p>

<div class="highlight highlight-source-cs"><pre><span class="pl-k">public</span> <span class="pl-k">void</span> Run()
{
  <span class="pl-k">var</span> actions = <span class="pl-k">new</span> List&lt;Action&gt;();
  <span class="pl-k">foreach</span> (<span class="pl-k">var</span> i <span class="pl-k">in</span> Enumerable.Range(<span class="pl-c1">0</span>, <span class="pl-c1">3</span>))
    actions.Add(() =&gt; Console.WriteLine(i));
  <span class="pl-k">foreach</span> (<span class="pl-k">var</span> action <span class="pl-k">in</span> actions)
    action();
}</pre></div>

<p>What will the code output? Sorry to say that there is no simple answer to this question. The matter is that earlier versions of C#, behavior of foreach was equal to behavior of for: variable of the cycle was created once and was captured in all lambdas. Starting from C# 5.0 this behavior has changed (<a href="http://blogs.msdn.com/b/ericlippert/archive/2009/11/12/closing-over-the-loop-variable-considered-harmful.aspx">here</a> Eric Lippert admits that Microsoft made the breaking change). Now this code outputs <code>"0 1 2"</code>. Note that this is a peculiarity of language, not of the platform. If you work in Visual Studio 2012 and change target framework to 3.5, nothing will change. And you will be able to see the old behavior in Visual Studio 2010. John Skit <a href="http://stackoverflow.com/questions/16264289/captured-closure-loop-variable-in-c-sharp-5-0">explains</a> why it was decided to make different behavior for <code>foreach</code> and <code>for</code>. Let’s have a look at a new variant of the expanded version of the code:</p>

<div class="highlight highlight-source-cs"><pre><span class="pl-k">public</span> <span class="pl-k">void</span> Run()
{
  <span class="pl-k">var</span> actions = <span class="pl-k">new</span> List&lt;Action&gt;();
  <span class="pl-k">foreach</span> (<span class="pl-k">int</span> i <span class="pl-k">in</span> Enumerable.Range(<span class="pl-c1">0</span>, <span class="pl-c1">3</span>))
  {
    DisplayClass c = <span class="pl-k">new</span> DisplayClass();
    с.i = i;    
    list.Add(c1.Action);
  }
  <span class="pl-k">foreach</span> (Action action <span class="pl-k">in</span> list)
    action();
}

<span class="pl-k">private</span> <span class="pl-k">sealed</span> <span class="pl-k">class</span> <span class="pl-en">DisplayClass</span>
{
  <span class="pl-k">public</span> <span class="pl-k">int</span> i;

  <span class="pl-k">public</span> <span class="pl-k">void</span> <span class="pl-en">Action</span>()
  {
    Console.WriteLine(i);
  }
}</pre></div>

<p>You can easily see the difference: in C# 5.0, for every iteration of the foreach cycle, we get a new instance of the generated class providing closure logic.</p>

<h3><a id="user-content-closure-of-multiple-variables" class="anchor" href="#closure-of-multiple-variables" aria-hidden="true"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Closure of multiple variables</h3>

<p>Let’s review a case when we get multiple variables that are captured in different variables:</p>

<div class="highlight highlight-source-cs"><pre><span class="pl-k">public</span> <span class="pl-k">void</span> Run()
{
  <span class="pl-k">int</span> x = <span class="pl-c1">1</span>, y = <span class="pl-c1">2</span>;
  Foo(u =&gt; u + x, u =&gt; u + y);
}</pre></div>

<p>One can think that in this case two additional classes are generated and each of them will be responsible for a single variable. Actually, a single class will be generated:</p>

<div class="highlight highlight-source-cs"><pre><span class="pl-k">public</span> <span class="pl-k">void</span> Run()
{
  DisplayClass с = <span class="pl-k">new</span> DisplayClass();
  с.x = <span class="pl-c1">1</span>;
  с.y = <span class="pl-c1">2</span>;
  Foo(с.ActionX, c.ActionY);
}

<span class="pl-k">private</span> <span class="pl-k">sealed</span> <span class="pl-k">class</span> <span class="pl-en">DisplayClass</span>
{
  <span class="pl-k">public</span> <span class="pl-k">int</span> x;
  <span class="pl-k">public</span> <span class="pl-k">int</span> y;

  <span class="pl-k">public</span> <span class="pl-k">int</span> <span class="pl-en">ActionX</span>(<span class="pl-k">int</span> <span class="pl-smi">u</span>)
  {
    <span class="pl-k">return</span> u + x;
  }

  <span class="pl-k">public</span> <span class="pl-k">int</span> <span class="pl-en">ActionY</span>(<span class="pl-k">int</span> <span class="pl-smi">u</span>)
  {
    <span class="pl-k">return</span> u + y;
  }
}</pre></div>

<p>Thus, lambdas are bound; garbage collector will access them when no reference to either of them will remain. Imagine the situation when the first lambda is used when initiating a long-living object, the second one is used when completing the work with it. And let there be many such objects. In this case initializing lambdas will stay in memory for quite a long time, though no one will ever invoke them.</p>

<h3><a id="user-content-scope" class="anchor" href="#scope" aria-hidden="true"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Scope</h3>

<p>There is one more peculiarity of closures that you need to know. Let’s review a sample:</p>

<div class="highlight highlight-source-cs"><pre><span class="pl-k">public</span> <span class="pl-k">void</span> Run(List&lt;<span class="pl-k">int</span>&gt; list)
{
  <span class="pl-k">foreach</span> (<span class="pl-k">var</span> element <span class="pl-k">in</span> list)
  {
    <span class="pl-k">var</span> e = element;
    <span class="pl-k">if</span> (Condition(e))
      Foo(x =&gt; x + e);
  }
}</pre></div>

<p>And here is the question: where the closure object will be created? In spite the lambda is created inside <code>if</code>, the object will be created in the same scope the captured variable is located in.</p>

<div class="highlight highlight-source-cs"><pre><span class="pl-k">public</span> <span class="pl-k">void</span> Run(List&lt;<span class="pl-k">int</span>&gt; list)
{
  <span class="pl-k">foreach</span> (<span class="pl-k">int</span> element <span class="pl-k">in</span> list)
  {
    DisplayClass c = <span class="pl-k">new</span> DisplayClass();
    c.e = element;
    <span class="pl-k">if</span> (Condition(c.e))
      Foo(c.Action);
  }
}

<span class="pl-k">private</span> <span class="pl-k">sealed</span> <span class="pl-k">class</span> <span class="pl-en">DisplayClass</span>
{
  <span class="pl-k">public</span> <span class="pl-k">int</span> e;

  <span class="pl-k">public</span> <span class="pl-k">int</span> <span class="pl-en">Action</span>(<span class="pl-k">int</span> <span class="pl-smi">x</span>)
  {
    <span class="pl-k">return</span> x + e;
  }
}</pre></div>

<p>This peculiarity is important when the <code>list</code> is quite big and the <code>Condition(e)</code> is executed quite rarely. <code>DisplayClass</code> instances will be created uselessly. It will affect memory and performance. We can fix the situation:</p>

<div class="highlight highlight-source-cs"><pre><span class="pl-k">public</span> <span class="pl-k">void</span> Run(List&lt;<span class="pl-k">int</span>&gt; list)
{
  <span class="pl-k">foreach</span> (<span class="pl-k">var</span> element <span class="pl-k">in</span> list)
    <span class="pl-k">if</span> (Condition(element))
    {
      <span class="pl-k">var</span> e = element;
      Foo(x =&gt; x + e);
    }
}</pre></div>

<p>This method will be deployed in a more optimal manner since <code>DisplayClass</code> constructor will be invoked when it is really necessary:</p>

<div class="highlight highlight-source-cs"><pre><span class="pl-k">public</span> <span class="pl-k">void</span> Run(List&lt;<span class="pl-k">int</span>&gt; list)
{
  <span class="pl-k">foreach</span> (<span class="pl-k">int</span> element <span class="pl-k">in</span> list)
    <span class="pl-k">if</span> (Condition(element))
    {
      DisplayClass c = <span class="pl-k">new</span> DisplayClass();
      c.e = element;
      Foo(c.Action);
    }  
}

<span class="pl-k">private</span> <span class="pl-k">sealed</span> <span class="pl-k">class</span> <span class="pl-en">DisplayClass</span>
{
  <span class="pl-k">public</span> <span class="pl-k">int</span> e;

  <span class="pl-k">public</span> <span class="pl-k">int</span> <span class="pl-en">Action</span>(<span class="pl-k">int</span> <span class="pl-smi">x</span>)
  {
    <span class="pl-k">return</span> x + e;
  }
}</pre></div>

<h3><a id="user-content-problems" class="anchor" href="#problems" aria-hidden="true"><svg aria-hidden="true" class="octicon octicon-link" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Problems</h3>

<p>You can find problems about the subject in <a href="http://problembook.net">ProblemBook.NET</a>:
<a href="http://problembook.net/content/en/Linq/ClosureAndForeach-P.html">ClosureAndForeach</a>,
<a href="http://problembook.net/content/en/Linq/ClosureAndFor-P.html">ClosureAndFor</a>,
<a href="http://problembook.net/content/en/Linq/ClosureAndVariable-P.html">ClosureAndVariable</a>.</p>

<hr>

<p>It’s very convenient to use the following utility: <a href="http://www.jetbrains.com/decompiler/">dotPeek</a> from <a href="http://www.jetbrains.com/">JetBrains</a> with enabled option <em>Show compiler-generated code</em>. Code included in the article is simplified in comparison with the disassembled version to make it easy to read.</p>
</article>
  </div>

</div>

<button type="button" data-facebox="#jump-to-line" data-facebox-class="linejump" data-hotkey="l" class="hidden">Jump to Line</button>
<div id="jump-to-line" style="display:none">
  <!-- </textarea> --><!-- '"` --><form accept-charset="UTF-8" action="" class="js-jump-to-line-form" method="get"><div style="margin:0;padding:0;display:inline"><input name="utf8" type="hidden" value="&#x2713;" /></div>
    <input class="form-control linejump-input js-jump-to-line-field" type="text" placeholder="Jump to line&hellip;" aria-label="Jump to line" autofocus>
    <button type="submit" class="btn">Go</button>
</form></div>

  </div>
  <div class="modal-backdrop js-touch-events"></div>
</div>


    </div>
  </div>

    </div>

        <div class="container site-footer-container">
  <div class="site-footer" role="contentinfo">
    <ul class="site-footer-links right">
        <li><a href="https://github.com/contact" data-ga-click="Footer, go to contact, text:contact">Contact GitHub</a></li>
      <li><a href="https://developer.github.com" data-ga-click="Footer, go to api, text:api">API</a></li>
      <li><a href="https://training.github.com" data-ga-click="Footer, go to training, text:training">Training</a></li>
      <li><a href="https://shop.github.com" data-ga-click="Footer, go to shop, text:shop">Shop</a></li>
        <li><a href="https://github.com/blog" data-ga-click="Footer, go to blog, text:blog">Blog</a></li>
        <li><a href="https://github.com/about" data-ga-click="Footer, go to about, text:about">About</a></li>

    </ul>

    <a href="https://github.com" aria-label="Homepage" class="site-footer-mark" title="GitHub">
      <svg aria-hidden="true" class="octicon octicon-mark-github" height="24" version="1.1" viewBox="0 0 16 16" width="24"><path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0 0 16 8c0-4.42-3.58-8-8-8z"></path></svg>
</a>
    <ul class="site-footer-links">
      <li>&copy; 2016 <span title="0.08736s from github-fe157-cp1-prd.iad.github.net">GitHub</span>, Inc.</li>
        <li><a href="https://github.com/site/terms" data-ga-click="Footer, go to terms, text:terms">Terms</a></li>
        <li><a href="https://github.com/site/privacy" data-ga-click="Footer, go to privacy, text:privacy">Privacy</a></li>
        <li><a href="https://github.com/security" data-ga-click="Footer, go to security, text:security">Security</a></li>
        <li><a href="https://status.github.com/" data-ga-click="Footer, go to status, text:status">Status</a></li>
        <li><a href="https://help.github.com" data-ga-click="Footer, go to help, text:help">Help</a></li>
    </ul>
  </div>
</div>



    

    <div id="ajax-error-message" class="ajax-error-message flash flash-error">
      <svg aria-hidden="true" class="octicon octicon-alert" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M8.865 1.52c-.18-.31-.51-.5-.87-.5s-.69.19-.87.5L.275 13.5c-.18.31-.18.69 0 1 .19.31.52.5.87.5h13.7c.36 0 .69-.19.86-.5.17-.31.18-.69.01-1L8.865 1.52zM8.995 13h-2v-2h2v2zm0-3h-2V6h2v4z"></path></svg>
      <button type="button" class="flash-close js-flash-close js-ajax-error-dismiss" aria-label="Dismiss error">
        <svg aria-hidden="true" class="octicon octicon-x" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M7.48 8l3.75 3.75-1.48 1.48L6 9.48l-3.75 3.75-1.48-1.48L4.52 8 .77 4.25l1.48-1.48L6 6.52l3.75-3.75 1.48 1.48z"></path></svg>
      </button>
      You can't perform that action at this time.
    </div>


      
      <script crossorigin="anonymous" integrity="sha256-ZVMF8EU/Qk+Nq4fcGuDjheWYL3L34s4U6xqcIpF0zr4=" src="https://assets-cdn.github.com/assets/frameworks-655305f0453f424f8dab87dc1ae0e385e5982f72f7e2ce14eb1a9c229174cebe.js"></script>
      <script async="async" crossorigin="anonymous" integrity="sha256-Ap4gZ0QqZAhCk0wu7qsrpYPS013Rh25dnA6gkrWv4WU=" src="https://assets-cdn.github.com/assets/github-029e2067442a640842934c2eeeab2ba583d2d35dd1876e5d9c0ea092b5afe165.js"></script>
      
      
      
      
      
      
    <div class="js-stale-session-flash stale-session-flash flash flash-warn flash-banner hidden">
      <svg aria-hidden="true" class="octicon octicon-alert" height="16" version="1.1" viewBox="0 0 16 16" width="16"><path d="M8.865 1.52c-.18-.31-.51-.5-.87-.5s-.69.19-.87.5L.275 13.5c-.18.31-.18.69 0 1 .19.31.52.5.87.5h13.7c.36 0 .69-.19.86-.5.17-.31.18-.69.01-1L8.865 1.52zM8.995 13h-2v-2h2v2zm0-3h-2V6h2v4z"></path></svg>
      <span class="signed-in-tab-flash">You signed in with another tab or window. <a href="">Reload</a> to refresh your session.</span>
      <span class="signed-out-tab-flash">You signed out in another tab or window. <a href="">Reload</a> to refresh your session.</span>
    </div>
    <div class="facebox" id="facebox" style="display:none;">
  <div class="facebox-popup">
    <div class="facebox-content" role="dialog" aria-labelledby="facebox-header" aria-describedby="facebox-description">
    </div>
    <button type="button" class="facebox-close js-facebox-close" aria-label="Close modal">
      <svg aria-hidden="true" class="octicon octicon-x" height="16" version="1.1" viewBox="0 0 12 16" width="12"><path d="M7.48 8l3.75 3.75-1.48 1.48L6 9.48l-3.75 3.75-1.48-1.48L4.52 8 .77 4.25l1.48-1.48L6 6.52l3.75-3.75 1.48 1.48z"></path></svg>
    </button>
  </div>
</div>

  </body>
</html>

