通过判断浏览器窗口是否失去焦点来判断有没有安装APP，如果有安装的话，会调起APP，窗口会失去焦点。
如果没有安装APP的话，焦点还在该窗口上，然后设定一个时间，超时即打开下载页。



function checkOutApp() {

       var isBlur = false;

        // 通过URL scheme来调起APP

        location.href = 'APP的URL scheme';

        setTimeout(function() {

            if (!isBlur) {

                location.href = 'APP的下载链接';

            }

        }, 1000);

        // window 每次失去焦點

        window.onblur = function() {

            console.log('失去焦點');

            isBlur = true;

        };

        var hiddenProperty = 'hidden' in document ? 'hidden' :

            'webkitHidden' in document ? 'webkitHidden' :

            'mozHidden' in document ? 'mozHidden' :

            null;

        var visibilityChangeEvent = hiddenProperty.replace(/hidden/i, 'visibilitychange');

        var onVisibilityChange = function() {

            if (document[hiddenProperty]) {

                console.log('失去焦點');

                isBlur = true;

            }

        }

        document.addEventListener(visibilityChangeEvent, onVisibilityChange);

    }
