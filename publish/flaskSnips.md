## FlaskSnips

```py
from flask import Flask, render_template_string, send_from_directory
from flask_cors import CORS

app = Flask(__name__, static_folder='charting_library/static')
CORS(app)

@app.route('/charting_library/<path:filename>')
def charting_library_files(filename):
    return send_from_directory('charting_library', filename)

```

```

```html
<script type="text/javascript" src="/charting_library/charting_library.standalone.js"></script>

<div id="tv_chart_container"></div>

  <script>

  </script>



```