##Protect your .htaccess file

    #Protect .htaccess
    <files ~ "^.*\.([Hh][Tt][Aa])">
    order allow,deny
    deny from all
    satisfy all
    </files>
    
 ##Protect your wp-config.php file

    # Protect wp-config.php
    <files wp-config.php>
    order allow,deny
    deny from all
    </files>
    
##Protect your error_log file

    #Protect error_log
    <files error_log>
    order allow,deny
    deny from all
    </files>

##Protect your WordPress Website from SQL Injection

    #Protect from SQL Injection
    Options +FollowSymLinks
    RewriteEngine On
    RewriteCond %{QUERY_STRING} (< |%3C).*script.*(>|%3E) [NC,OR]
    RewriteCond %{QUERY_STRING} GLOBALS(=|[|%[0-9A-Z]{0,2}) [OR]
    RewriteCond %{QUERY_STRING} _REQUEST(=|[|%[0-9A-Z]{0,2})
    RewriteRule ^(.*)$ index.php [F,L]
    
##Prevent others from Hotlinking your Pictures.

    # Disable hotlinking of images
    RewriteEngine on
    RewriteCond %{HTTP_REFERER} !^$
    RewriteCond %{HTTP_REFERER} !^http(s)?://(www\.)?example.com [NC]
    RewriteCond %{HTTP_REFERER} !^http(s)?://(www\.)?google.com [NC]
    RewriteRule \.(jpg|jpeg|png|gif)$ – [NC,F,L]
    
##Restrict Direct Access to Plugin and Theme PHP files

    # Restrict Direct Access to Plugin and Theme PHP files
    RewriteCond %{REQUEST_URI} !^/wp-content/plugins/file/to/exclude\.php
    RewriteCond %{REQUEST_URI} !^/wp-content/plugins/directory/to/exclude/
    RewriteRule wp-content/plugins/(.*\.php)$ - [R=404,L]
    RewriteCond %{REQUEST_URI} !^/wp-content/themes/file/to/exclude\.php
    RewriteCond %{REQUEST_URI} !^/wp-content/themes/directory/to/exclude/
    RewriteRule wp-content/themes/(.*\.php)$ - [R=404,L]
    
##Secure the wp-includes Directory

    # Protect Include-Only files
    <ifmodule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule ^wp-admin/includes/ - [F,L]
    RewriteRule !^wp-includes/ - [S=3]
    RewriteRule ^wp-includes/[^/]+\.php$ - [F,L]
    RewriteRule ^wp-includes/js/tinymce/langs/.+\.php - [F,L]
    RewriteRule ^wp-includes/theme-compat/ - [F,L]
    </ifmodule>
    
 ##Don’t Let People Browse your Directories

    # Disable directory browsing
    Options All -Indexes
    
 ##Block Author Scans

    # BEGIN block author scans
    RewriteEngine On
    RewriteBase /
    RewriteCond %{QUERY_STRING} (author=\d+) [NC]
    RewriteRule .* - [F]
    # END block author scans
    
    
##Block WordPress XMLRPC Requests

    # Block WordPress xmlrpc.php requests
    <files xmlrpc.php>
    order deny,allow
    deny from all
    </files>
    
##6G Firewall/Blacklist

    # 6G FIREWALL/BLACKLIST
    # @ https://perishablepress.com/6g/

    # 6G:[QUERY STRINGS]
    <IfModule mod_rewrite.c>
     RewriteEngine On
     RewriteCond %{QUERY_STRING} (eval\() [NC,OR]
     RewriteCond %{QUERY_STRING} (127\.0\.0\.1) [NC,OR]
     RewriteCond %{QUERY_STRING} ([a-z0-9]{2000,}) [NC,OR]
     RewriteCond %{QUERY_STRING} (javascript:)(.*)(;) [NC,OR]
     RewriteCond %{QUERY_STRING} (base64_encode)(.*)(\() [NC,OR]
     RewriteCond %{QUERY_STRING} (GLOBALS|REQUEST)(=|\[|%) [NC,OR]
     RewriteCond %{QUERY_STRING} (<|%3C)(.*)script(.*)(>|%3) [NC,OR]
     RewriteCond %{QUERY_STRING} (\\|\.\.\.|\.\./|~|`|<|>|\|) [NC,OR]
     RewriteCond %{QUERY_STRING} (boot\.ini|etc/passwd|self/environ) [NC,OR]
     RewriteCond %{QUERY_STRING} (thumbs?(_editor|open)?|tim(thumb)?)\.php [NC,OR]
     RewriteCond %{QUERY_STRING} (\'|\")(.*)(drop|insert|md5|select|union) [NC]
     RewriteRule .* - [F]
    </IfModule>

    # 6G:[REQUEST METHOD]
    <IfModule mod_rewrite.c>
     RewriteCond %{REQUEST_METHOD} ^(connect|debug|move|put|trace|track) [NC]
     RewriteRule .* - [F]
    </IfModule>

    # 6G:[REFERRERS]
    <IfModule mod_rewrite.c>
     RewriteCond %{HTTP_REFERER} ([a-z0-9]{2000,}) [NC,OR]
     RewriteCond %{HTTP_REFERER} (semalt.com|todaperfeita) [NC]
     RewriteRule .* - [F]
    </IfModule>

    # 6G:[REQUEST STRINGS]
    <IfModule mod_alias.c>
     RedirectMatch 403 (?i)([a-z0-9]{2000,})
     RedirectMatch 403 (?i)(https?|ftp|php):/
     RedirectMatch 403 (?i)(base64_encode)(.*)(\()
     RedirectMatch 403 (?i)(=\\\'|=\\%27|/\\\'/?)\.
     RedirectMatch 403 (?i)/(\$(\&)?|\*|\"|\.|,|&|&amp;?)/?$
     RedirectMatch 403 (?i)(\{0\}|\(/\(|\.\.\.|\+\+\+|\\\"\\\")
     RedirectMatch 403 (?i)(~|`|<|>|:|;|,|%|\\|\s|\{|\}|\[|\]|\|)
     RedirectMatch 403 (?i)/(=|\$&|_mm|cgi-|etc/passwd|muieblack)
     RedirectMatch 403 (?i)(&pws=0|_vti_|\(null\)|\{\$itemURL\}|echo(.*)kae|etc/passwd|eval\(|self/environ)
     RedirectMatch 403 (?i)\.(aspx?|bash|bak?|cfg|cgi|dll|exe|git|hg|ini|jsp|log|mdb|out|sql|svn|swp|tar|rar|rdf)$
     RedirectMatch 403 (?i)/(^$|(wp-)?config|mobiquo|phpinfo|shell|sqlpatch|thumb|thumb_editor|thumbopen|timthumb|webshell)\.php
    </IfModule>

    # 6G:[USER AGENTS]
    <IfModule mod_setenvif.c>
     SetEnvIfNoCase User-Agent ([a-z0-9]{2000,}) bad_bot
     SetEnvIfNoCase User-Agent (archive.org|binlar|casper|checkpriv|choppy|clshttp|cmsworld|diavol|dotbot|extract|feedfinder|flicky|g00g1e|harvest|heritrix|httrack|kmccrew|loader|miner|nikto|nutch|planetwork|postrank|purebot|pycurl|python|seekerspider|siclab|skygrid|sqlmap|sucker|turnit|vikspider|winhttp|xxxyy|youda|zmeu|zune) bad_bot

     # Apache < 2.3
     <IfModule !mod_authz_core.c>
      Order Allow,Deny
      Allow from all
      Deny from env=bad_bot
     </IfModule>

     # Apache >= 2.3
     <IfModule mod_authz_core.c>
      <RequireAll>
       Require all Granted
       Require not env bad_bot
      </RequireAll>
     </IfModule>
    </IfModule>

    # 6G:[BAD IPS]
    <Limit GET HEAD OPTIONS POST PUT>
     Order Allow,Deny
     Allow from All
     # uncomment/edit/repeat next line to block IPs
     # Deny from 123.456.789
    </Limit>
    
##HackRepairs Blacklist

   # Start HackRepair.com Blacklist
    RewriteEngine on
    # Start Abuse Agent Blocking
    RewriteCond %{HTTP_USER_AGENT} "^Mozilla.*Indy" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Mozilla.*NEWT" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^$" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Maxthon$" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^SeaMonkey$" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Acunetix" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^binlar" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^BlackWidow" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Bolt 0" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^BOT for JCE" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Bot mailto\:craftbot@yahoo\.com" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^casper" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^checkprivacy" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^ChinaClaw" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^clshttp" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^cmsworldmap" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Custo" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Default Browser 0" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^diavol" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^DIIbot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^DISCo" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^dotbot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Download Demon" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^eCatch" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^EirGrabber" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^EmailCollector" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^EmailSiphon" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^EmailWolf" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Express WebPictures" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^extract" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^ExtractorPro" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^EyeNetIE" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^feedfinder" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^FHscan" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^FlashGet" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^flicky" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^g00g1e" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^GetRight" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^GetWeb\!" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Go\!Zilla" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Go\-Ahead\-Got\-It" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^grab" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^GrabNet" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Grafula" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^harvest" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^HMView" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Image Stripper" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Image Sucker" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^InterGET" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Internet Ninja" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^InternetSeer\.com" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^jakarta" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Java" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^JetCar" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^JOC Web Spider" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^kanagawa" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^kmccrew" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^larbin" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^LeechFTP" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^libwww" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Mass Downloader" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^microsoft\.url" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^MIDown tool" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^miner" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Mister PiX" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^MSFrontPage" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Navroad" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^NearSite" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Net Vampire" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^NetAnts" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^NetSpider" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^NetZIP" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^nutch" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Octopus" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Offline Explorer" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Offline Navigator" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^PageGrabber" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Papa Foto" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^pavuk" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^pcBrowser" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^PeoplePal" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^planetwork" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^psbot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^purebot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^pycurl" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^RealDownload" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^ReGet" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Rippers 0" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^sitecheck\.internetseer\.com" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^SiteSnagger" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^skygrid" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^SmartDownload" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^sucker" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^SuperBot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^SuperHTTP" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Surfbot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^tAkeOut" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Teleport Pro" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Toata dragostea mea pentru diavola" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^turnit" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^vikspider" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^VoidEYE" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Web Image Collector" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WebAuto" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WebBandit" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WebCopier" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WebFetch" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WebGo IS" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WebLeacher" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WebReaper" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WebSauger" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Website eXtractor" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Website Quester" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WebStripper" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WebWhacker" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WebZIP" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Widow" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WPScan" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WWW\-Mechanize" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^WWWOFFLE" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Xaldon WebSpider" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^Zeus" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "^zmeu" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "360Spider" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "CazoodleBot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "discobot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "EasouSpider" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "ecxi" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "GT\:\:WWW" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "heritrix" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "HTTP\:\:Lite" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "HTTrack" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "ia_archiver" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "id\-search" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "IDBot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "Indy Library" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "IRLbot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "ISC Systems iRc Search 2\.1" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "LinksCrawler" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "LinksManager\.com_bot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "linkwalker" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "lwp\-trivial" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "MFC_Tear_Sample" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "Microsoft URL Control" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "Missigua Locator" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "MJ12bot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "panscient\.com" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "PECL\:\:HTTP" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "PHPCrawl" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "PleaseCrawl" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "SBIder" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "SearchmetricsBot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "SeznamBot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "Snoopy" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "Steeler" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "URI\:\:Fetch" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "urllib" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "Web Sucker" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "webalta" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "WebCollage" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "Wells Search II" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "WEP Search" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "XoviBot" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "YisouSpider" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "zermelo" [NC,OR]
    RewriteCond %{HTTP_USER_AGENT} "ZyBorg" [NC,OR]
    # End Abuse Agent Blocking
    # Start Abuse HTTP Referrer Blocking
    RewriteCond %{HTTP_REFERER} "^https?://(?:[^/]+\.)?semalt\.com" [NC,OR]
    RewriteCond %{HTTP_REFERER} "^https?://(?:[^/]+\.)?kambasoft\.com" [NC,OR]
    RewriteCond %{HTTP_REFERER} "^https?://(?:[^/]+\.)?savetubevideo\.com" [NC]
    # End Abuse HTTP Referrer Blocking
    RewriteRule ^.* - [F,L]
    # End HackRepair.com Blacklist, http://pastebin.com/u/hackrepair


#Useful links :
-https://www.pixemweb.com/
-http://mdpabel.me
