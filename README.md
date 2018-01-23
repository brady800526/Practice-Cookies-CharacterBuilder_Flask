## Summary

This repo is a Treehouse Code provided by teacher in Treehouse. We use cookies to save user choices. 

*All the step is followed by [Flask Basics Course](https://www.youtube.com/watch?v=eBwhBrNbrNI&list=PLXmMXHVSvS-CMpHUeyIeqzs3kl-tIG-8R&index=3) created by Kenneth Love.*

## Cookies

Cookies is a dictionary stored in the web stroage, we can use cookies to save user's previous decisions in order to prevent user accidently quit the app, or we can use cookies to provide user more convenient usage, like auto fill in the form for them.

## Usage

Download the repo, under the directory run:

    python app.py

You will see the landing page.

<figure style="text-align: center;">
    <img src="http://i65.tinypic.com/xd9hrl.png" alt="Landing Page" style="width: 60%; height: 60%"/>
    <figcaption style="display: block;">Landing Page</figcaption>
</figure>

By entering name "Brady", then press 【Let's build it】. User will be send to the builder page.

<figure style="text-align: center;">
    <img src="http://i68.tinypic.com/oizjts.png" alt="Builder Page" style="width: 60%; height: 60%"/>
    <figcaption style="display: block;">Builder Page</figcaption>
</figure>

One we select the clothes you want the bear to dress on, and press the 【Updata】button. The bear will dress up the new clothes.

<figure style="text-align: center;">
    <img src="http://i68.tinypic.com/2a4o7rl.png" alt="Builder Page" style="width: 60%; height: 60%"/>
    <figcaption style="display: block;">Builder Page</figcaption>
</figure>

## Architecture

All the logic are placed in the app.py, so we will majorly discuss the app.py.

In the landing page (index.html), we send the username and press the 【Let's build it】button. Then we redirect to builder page (builder.html), you can see your name on the left top corner. At the mean time, you can put the clothes on the bear as you want.

<figure style="text-align: center;">
    <img src="http://i63.tinypic.com/wlzsqh.jpg" alt="The architecture of this app" style="width: 80%; height: 80%"/>
    <figcaption style="display: block;">The architecture of this app</figcaption>
</figure>

## Code

#### Setting
------------------------------------------------------------------------

app.py

```
app = Flask(__name__)
```

First we start an app initialized by Flask Framework `app = Flask(__name__)`

### get_saved_data function - Load the cookies
------------------------------------------------------------------------

app.py
```
try:
    data = json.loads(request.cookies.get('character'))
except TypeError:
    data = {}
return data
```

If there is cookies in the website, load it to data. Otherwise the data will be empty.

#### index route - Show the Landing Page
------------------------------------------------------------------------

app.py
```
@app.route('/')
def index():
    return render_template('index.html', saves=get_saved_data())
```

This route will enter the landing page, and if there is cookies, pass it into landing page with name 'saves'.

#### save route - Save the form information
------------------------------------------------------------------------

app.py
```
@app.route('/save', methods=['POST'])
def save():
    flash("Alright! That looks awesome!")
    response = make_response(redirect(url_for('builder')))
    data = get_saved_data()
    data.update(dict(request.form.items()))
    response.set_cookie('character', json.dumps(data))
    return response
```

If it is POST request, this route will flash a string show you was updated. Besides, update the form data includes all the from from both builder page and landing page.

#### builder route - Show the character
------------------------------------------------------------------------

app.py
```
def builder():
    return render_template(
        'builder.html',
        saves=get_saved_data(),
        options=DEFAULTS
    )
```

Return the builder page, pass cookies and DEFAULTS to the builder page.

#### index page - Enter the name of bear
------------------------------------------------------------------------

index.html - L9~L12
```
<form action="{{ url_for('save') }}" method="POST">
    <label>Name your bear</label>
        <input type="text" name="name" value="{{ saves.get('name', '') }}" autofocus>
        <input type="submit" value="Let's build it!">
</form>
```

This form will be sent to 'save' view function after press submit. All the data sent is in dictinoary form, in this case

```
<input type="text" name="name" value="{{ saves.get('name', '') }}" autofocus>
```

will send dictionary {"name", "saves.get('name', '')} to the 'save' view function

#### builder page - Modify the dress of the bear
------------------------------------------------------------------------

index.html - L5
```
<form action="{{ url_for('save') }}" method="POST" class="wrap no-top">
```

Notice that almost all the file is form, so it will be sent to 'save' view function.

index.html - L49~L56
```
<div class="grid-80">
    <input type="radio" id="no_{{ category }}_icon" name="{{ category }}" value='' {% if not saves.get(category) %}checked{% endif %}>
    <label for="no_{{ category }}_icon"><img src="/static/img/no-selection.svg"></label>
    {% for choice in choices %}
    <input type="radio" id="{{ category }}-{{ choice }}_icon" name="{{ category }}" value="{{ choice }}" {% if saves.get(category) == choice %}checked{% endif %}>
    <label for="{{ category }}-{{ choice }}_icon"><img src="/static/img/{{ category }}-{{ choice }}.svg"></label>
    {% endfor %}
</div>
```

This part will send the name-value dictionary in the html tag and send to 'save' view function.

index.html - L22~L41
```
<div id="bear" class="grid-100 bg-{{ saves.get('colors') }}">
    <div class="bear-body"><img src="/static/img/bear_body.svg" /></div>
    {% if saves.get('footwear') %}
    <div class="footwear"><img src="/static/img/bear_items_footwear-{{ saves['footwear'] }}.svg"></div>
    {% endif %}
    {% if saves.get('pants') %}
    <div class="pants"><img src="/static/img/bear_items_pants-{{ saves['pants'] }}.svg"></div>
    {% endif %}
    {% if saves.get('shirts') %}
    <div class="shirt"><img src="/static/img/bear_items_shirts-{{ saves['shirts'] }}.svg"></div>
    {% endif %}
    <div class="head"><img src="/static/img/bear_face.svg" /></div>
    {% if saves.get('glasses') %}
    <div class="glasses"><img src="/static/img/bear_items_glasses-{{ saves['glasses'] }}.svg"></div>
    {% endif %}
    <div class="nose"><img src="/static/img/bear_nose.svg" /></div>
    {% if saves.get('hat') %}
    <div class="hat"><img src="/static/img/bear_items_hat-{{ saves['hat'] }}.svg"></div>
    {% endif %}
</div>
```

Then if there is cookies of what you selected before, if will dress up the clothes on bear.

## Conclusion

In this project, we use cookies to save user's enter name and update the bear clothes. Frankly speaking, the most time-consuming part for this project is that you the UI design part LOL.
