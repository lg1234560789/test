# test


<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>06_context使用</title>
</head>
<body>

<div id="test"></div>
<a id="call-app" href="javascript:;" > Start or Download </a><br/><br/>

<script type="text/javascript" src="./js/react.development.js"></script>
<script type="text/javascript" src="./js/react-dom.development.js"></script>
<script type="text/javascript" src="./js/prop-types.js"></script>
<script type="text/javascript" src="./js/babel.min.js"></script>
    <script type="text/javascript">
            (function(){
        var ua = navigator.userAgent.toLowerCase();
        var t;
        var config = {
                /*scheme:必须*/
                scheme_IOS: 'com.wntsvideo.com://',
                scheme_Adr: 'h5wants://wants.com/openwith?type=1&targetId=100469&targetTitle=aaa&targetContent=bbbb',
                download_url: 'http://a.app.qq.com/o/simple.jsp?pkgname=com.wants',
                timeout: 600
        };

        function openclient() {
            var startTime = Date.now();
            var ifr = document.createElement('iframe');
            ifr.src = ua.indexOf('os') > 0 ? config.scheme_IOS : config.scheme_Adr;
            ifr.style.display = 'none';
            document.body.appendChild(ifr);
            var t = setTimeout(function() {
                var endTime = Date.now();
                if (!startTime || endTime - startTime < config.timeout + 200) {
                    window.location = config.download_url;
                } else {
 
                }
            }, config.timeout);
            window.onblur = function() {
                clearTimeout(t);
            }
        }
        window.addEventListener("DOMContentLoaded", function(){
            document.getElementById("call-app").addEventListener('click',
                    openclient, false);
        }, false);
    })()
    </script>



</body>
</html>

