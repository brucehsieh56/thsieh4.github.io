---
title: MAIN page
---

test my main page

{% include header.html %}


[go to page01](https://thsieh4.github.io/page01/)

[go to page02](https://thsieh4.github.io/page02.html)

[go to page03](https://thsieh4.github.io/page03.html)

[go to boostrap page](https://thsieh4.github.io/bootstrap.html)


<myxml>
   <meta charset="utf-8">
   <title>my plot</title>
   
   <link rel="stylesheet" href="https://cdn.pydata.org/bokeh/release/bokeh-0.12.10.min.css" type="text/css" />
   <script type="text/javascript" src="https://cdn.pydata.org/bokeh/release/bokeh-0.12.10.min.js"></script>
   <script type="text/javascript">
      Bokeh.set_log_level("info");
   </script>
   
   <style>
          html {
            width: 100%;
            height: 100%;
          }
          body {
            width: 100%;
            height: 100%;
            margin: auto;
          }
   </style>
   
   <div class="bk-root">
      <div class="bk-plotdiv" id="80455f7d-4b49-43a2-9799-374cfd70749c"></div>
   </div>
   
   <script type="text/javascript">
            (function() {
          var fn = function() {
            Bokeh.safely(function() {
              (function(root) {
                function embed_document(root) {
                  var docs_json = {"86b26458-cbb8-45b4-b284-c01905206f8f":{"roots":{"references":[{"attributes":{},"id":"1e98ad1d-2c19-478c-b70a-10dc91dafd26","type":"BasicTickFormatter"},{"attributes":{"data_source":{"id":"ec963cba-2775-44a9-8342-2aa8b7da019b","type":"ColumnDataSource"},"glyph":{"id":"8ad6a639-93f0-4802-9ad4-14a2109a253e","type":"Circle"},"hover_glyph":null,"muted_glyph":null,"nonselection_glyph":{"id":"08ebf1f5-f952-48d6-8f0a-c4470adff134","type":"Circle"},"selection_glyph":null,"view":{"id":"286155eb-9aab-4182-b996-32f3c9f93af3","type":"CDSView"}},"id":"97f89401-2c21-431c-ad3b-d844d5df135e","type":"GlyphRenderer"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"#1f77b4"},"line_alpha":{"value":0.1},"line_color":{"value":"#1f77b4"},"x":{"field":"x"},"y":{"field":"y"}},"id":"08ebf1f5-f952-48d6-8f0a-c4470adff134","type":"Circle"},{"attributes":{"callback":null},"id":"e8c96ad7-7d77-4a2d-b79d-7a52e74a22ee","type":"DataRange1d"},{"attributes":{},"id":"4da50be3-c97b-4024-8a37-969e47a24af6","type":"PanTool"},{"attributes":{"fill_color":{"value":"#1f77b4"},"line_color":{"value":"#1f77b4"},"x":{"field":"x"},"y":{"field":"y"}},"id":"8ad6a639-93f0-4802-9ad4-14a2109a253e","type":"Circle"},{"attributes":{},"id":"65112f3d-69b8-4d01-aa2c-fcb09784ae27","type":"WheelZoomTool"},{"attributes":{"formatter":{"id":"7afa5bce-e454-4140-a9f6-31da8ebcbf16","type":"BasicTickFormatter"},"plot":{"id":"8e20afee-434a-41fc-b664-2a610620fc80","subtype":"Figure","type":"Plot"},"ticker":{"id":"db3401f5-3985-494a-92e0-53f617d0f09f","type":"BasicTicker"}},"id":"dc436c04-dd12-4e1d-bf07-4c8453f36ab3","type":"LinearAxis"},{"attributes":{"overlay":{"id":"e40e6331-3247-4314-91fe-d30fac82e70a","type":"BoxAnnotation"}},"id":"5d8c256a-7a95-4eda-85aa-460de975f41e","type":"BoxZoomTool"},{"attributes":{},"id":"3d1fc43c-c481-4bf9-93ec-6f9958466b6f","type":"SaveTool"},{"attributes":{},"id":"ec7ececb-b152-42f4-aa66-923c590daad2","type":"LinearScale"},{"attributes":{"below":[{"id":"dc436c04-dd12-4e1d-bf07-4c8453f36ab3","type":"LinearAxis"}],"left":[{"id":"1cb555f4-3ba9-4778-90da-666f7f4845e3","type":"LinearAxis"}],"renderers":[{"id":"dc436c04-dd12-4e1d-bf07-4c8453f36ab3","type":"LinearAxis"},{"id":"39204f4a-2141-4fde-85c5-60a0150473a4","type":"Grid"},{"id":"1cb555f4-3ba9-4778-90da-666f7f4845e3","type":"LinearAxis"},{"id":"15db6934-4e63-4c3b-a3fc-8e9319e1f177","type":"Grid"},{"id":"e40e6331-3247-4314-91fe-d30fac82e70a","type":"BoxAnnotation"},{"id":"97f89401-2c21-431c-ad3b-d844d5df135e","type":"GlyphRenderer"}],"title":{"id":"3a471e23-cdbf-4548-be8c-bba39450dd54","type":"Title"},"toolbar":{"id":"14e74cd8-e770-419b-912c-7c98a3f4df3b","type":"Toolbar"},"x_range":{"id":"59a81933-2d53-4242-9ad8-a133a95060da","type":"DataRange1d"},"x_scale":{"id":"ec7ececb-b152-42f4-aa66-923c590daad2","type":"LinearScale"},"y_range":{"id":"e8c96ad7-7d77-4a2d-b79d-7a52e74a22ee","type":"DataRange1d"},"y_scale":{"id":"74d58d96-9bf0-4a8e-a862-88c00a67fa84","type":"LinearScale"}},"id":"8e20afee-434a-41fc-b664-2a610620fc80","subtype":"Figure","type":"Plot"},{"attributes":{"source":{"id":"ec963cba-2775-44a9-8342-2aa8b7da019b","type":"ColumnDataSource"}},"id":"286155eb-9aab-4182-b996-32f3c9f93af3","type":"CDSView"},{"attributes":{},"id":"c9d97952-51e4-40c9-8e28-b91494350500","type":"ResetTool"},{"attributes":{"active_drag":"auto","active_inspect":"auto","active_scroll":"auto","active_tap":"auto","tools":[{"id":"4da50be3-c97b-4024-8a37-969e47a24af6","type":"PanTool"},{"id":"65112f3d-69b8-4d01-aa2c-fcb09784ae27","type":"WheelZoomTool"},{"id":"5d8c256a-7a95-4eda-85aa-460de975f41e","type":"BoxZoomTool"},{"id":"3d1fc43c-c481-4bf9-93ec-6f9958466b6f","type":"SaveTool"},{"id":"c9d97952-51e4-40c9-8e28-b91494350500","type":"ResetTool"},{"id":"6159ebf8-900a-4190-a115-bd4546cb8748","type":"HelpTool"}]},"id":"14e74cd8-e770-419b-912c-7c98a3f4df3b","type":"Toolbar"},{"attributes":{},"id":"6159ebf8-900a-4190-a115-bd4546cb8748","type":"HelpTool"},{"attributes":{"callback":null},"id":"59a81933-2d53-4242-9ad8-a133a95060da","type":"DataRange1d"},{"attributes":{"plot":null,"text":""},"id":"3a471e23-cdbf-4548-be8c-bba39450dd54","type":"Title"},{"attributes":{},"id":"7afa5bce-e454-4140-a9f6-31da8ebcbf16","type":"BasicTickFormatter"},{"attributes":{"callback":null,"column_names":["x","y"],"data":{"x":[1,2],"y":[3,4]}},"id":"ec963cba-2775-44a9-8342-2aa8b7da019b","type":"ColumnDataSource"},{"attributes":{},"id":"db3401f5-3985-494a-92e0-53f617d0f09f","type":"BasicTicker"},{"attributes":{"plot":{"id":"8e20afee-434a-41fc-b664-2a610620fc80","subtype":"Figure","type":"Plot"},"ticker":{"id":"db3401f5-3985-494a-92e0-53f617d0f09f","type":"BasicTicker"}},"id":"39204f4a-2141-4fde-85c5-60a0150473a4","type":"Grid"},{"attributes":{},"id":"74d58d96-9bf0-4a8e-a862-88c00a67fa84","type":"LinearScale"},{"attributes":{"formatter":{"id":"1e98ad1d-2c19-478c-b70a-10dc91dafd26","type":"BasicTickFormatter"},"plot":{"id":"8e20afee-434a-41fc-b664-2a610620fc80","subtype":"Figure","type":"Plot"},"ticker":{"id":"3f6eb753-1f31-4b52-b7fd-e5f3fb443e4e","type":"BasicTicker"}},"id":"1cb555f4-3ba9-4778-90da-666f7f4845e3","type":"LinearAxis"},{"attributes":{},"id":"3f6eb753-1f31-4b52-b7fd-e5f3fb443e4e","type":"BasicTicker"},{"attributes":{"dimension":1,"plot":{"id":"8e20afee-434a-41fc-b664-2a610620fc80","subtype":"Figure","type":"Plot"},"ticker":{"id":"3f6eb753-1f31-4b52-b7fd-e5f3fb443e4e","type":"BasicTicker"}},"id":"15db6934-4e63-4c3b-a3fc-8e9319e1f177","type":"Grid"},{"attributes":{"bottom_units":"screen","fill_alpha":{"value":0.5},"fill_color":{"value":"lightgrey"},"left_units":"screen","level":"overlay","line_alpha":{"value":1.0},"line_color":{"value":"black"},"line_dash":[4,4],"line_width":{"value":2},"plot":null,"render_mode":"css","right_units":"screen","top_units":"screen"},"id":"e40e6331-3247-4314-91fe-d30fac82e70a","type":"BoxAnnotation"}],"root_ids":["8e20afee-434a-41fc-b664-2a610620fc80"]},"title":"Bokeh Application","version":"0.12.10"}};
                  var render_items = [{"docid":"86b26458-cbb8-45b4-b284-c01905206f8f","elementid":"80455f7d-4b49-43a2-9799-374cfd70749c","modelid":"8e20afee-434a-41fc-b664-2a610620fc80"}];
                  root.Bokeh.embed.embed_items(docs_json, render_items);
                }
                if (root.Bokeh !== undefined) {
                  embed_document(root);
                } else {
                  var attempts = 0;
                  var timer = setInterval(function(root) {
                    if (root.Bokeh !== undefined) {
                      embed_document(root);
                      clearInterval(timer);
                    }
                    attempts++;
                    if (attempts > 100) {
                      console.log("Bokeh: ERROR: Unable to embed document because BokehJS library is missing")
                      clearInterval(timer);
                    }
                  }, 10, root)
                }
              })(window);
            });
          };
          if (document.readyState != "loading") fn();
          else document.addEventListener("DOMContentLoaded", fn);
        })();
        
   </script>
</myxml>
