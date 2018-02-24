# 多行文本的pc端的完美解决方案

常规思路



    .textHidden_p(@num, @height) {
      display: -webkit-box;
      -webkit-line-clamp: @num;
      -webkit-box-orient: vertical;
      line-height: @height;
      overflow: hidden;
      text-overflow: ellipsis;
    }
但是这种方式只支持webkit内核的浏览器，像火狐是不支持的

原因：line-clamp不是标准规范，只有webkit内核的浏览器支持

解决方案

    <div class="cpInfo">
     <p>
        的金额接军地多军诶军地军二多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多的金额接军地多军诶军地军二多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多多是是是是是是所所是是是是是是所所少shu1234
      </p>
    </div>


    window.dotted = function(classname,element){
            $(classname).each(function(i){
                var divH = $(this).height();
                var $p = $(this).find(element).eq(0);
                while ($p.outerHeight() > divH) {
                    var a= $p.text($p.text().replace(/(\s)*([a-zA-Z0-9]+|\W)(\.\.\.)?$/, "..."));
                };
            });
     };
    window.dotted(".cpInfo", 'p');
