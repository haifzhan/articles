# Render WeasyPrint Ticket Sample in Web & PDF and Write to PDF (Part 1)

## What is WeasyPrint and What I am going to do?
WeasyPrint is a python library which helps developers build beautiful PDF reports using HTML/CSS knowledge. You can
generate PDF file directly or render the PDF on web page using Flask. In this article I will show you how to use WeasyPrint
    
    - Render to Web Page
    - Render to PDF
    - Write to PDF

# My Story 
Last couple of months, I was working on a project that parses JSON input source and build PDF report(14 Pages). I did research on jsPDF, ReportLab and pyPDF(I am a Full Stack Dev and have Java/Python/JS background, it really doesn't matter which technology to use, the key point is the tool is easy to learn and can build nice report). I experienced each of them at least one week and built a tiny mockup with each of the technology, as I didn't have too much experience on them, the learning curve was high and I couldn't build beautiful report in a short time. Each of them has their own rules and it takes time to get used to. The report I built contains graphs and tables, those two elements increase the difficulties to build report using the mentioned tools.

The project was urgent and I need a tool which can build gorges report in a short time(a month or two). Finally I found WeasyPrint which heavily based on HTML/CSS which mean no new rules to learn and if you can mockup the report using HTML/CSS and it can be generated the PDF version.


# Steps to render in Web page and PDF
## Installation
`pip install WeasyPrint` and `pip install Flask-WeasyPrint`

## Preview:

### Render Web Page
![alt text](./images/render-weasyprint/render-web-page.png)

### Render PDF 
![alt text](./images/render-weasyprint/render-pdf.png)

## _Render Web Page & PDF_
### Folder Structure
Make sure use the structure which can be recognized by Flask.
```
ticket
  |__web
       |__static
              |__styles
                     |__ticket.css  
       |__templates
              |__ticket.html
       |__flask_ticket.py
       |__README.md
```

### Setup Flask and enable debug mode:
Imports ticket.css, add below lines into ticket.html 
```
<head>
    <link rel=stylesheet href="{{ url_for('static', filename='styles/ticket.css') }}" />
</head>
```

**flask_ticket.py**
Two routes here, one is render to web page directly and another is render to pdf on webpage.
```
from os import path, walk
from flask import Flask, render_template, url_for
from flask_weasyprint import HTML, render_pdf

app = Flask(__name__)

# Display Web View
@app.route('/ticket/', defaults={'name': 'World'})
def ticket_html(name):
    return render_template('ticket.html', name=name)


# Generate PDF Version
@app.route('/ticket.pdf')
def ticket_pdf():
    # Make a PDF straight from HTML in a string.
    name =''
    html = render_template('ticket.html', name=name)
    return render_pdf(HTML(string=html))
```

I am not going to paste the ticket.html and ticket.css here, you can find them from [WeasyPrint Samples](https://weasyprint.org/samples/)

Tells Flask which app file to use and enables debug mode which helps debugging
```
export FLASK_APP=flask_ticket.py
export FLASK_DEBUG=1
# run below command in the same folder of flask_ticket.py
/path/to/bin/flask run
```

go `localhost:5000/ticket` will render to webpage and `localhost:5000/ticket.pdf` will render to PDF.

## _Write to PDF_
Make sure comment out `<link rel=stylesheet href="{{ url_for('static', filename='styles/ticket.css') }}" />`
when write to PDF file.

pdf_ticket.py
```
import os
import weasyprint
from jinja2 import Environment, FileSystemLoader

SRC_DIR = '/path/to/ticket/sandbox'
DEST_DIR = '/path/to/ticket/sandbox'
TEMPLATE = 'ticket.html'  # download from WeasyPrint sample
CSS = 'ticket.css'  # download from WeasyPrint sample
OUTPUT_FILENAME = 'test-ticket.pdf'

def start():
    env = Environment(loader=FileSystemLoader(SRC_DIR))

    template = env.get_template(TEMPLATE)
    css = os.path.join(SRC_DIR, CSS)
    
    # variables
    template_vars = { }

    # rendering to html string
    rendered_string = template.render(template_vars)
    html = weasyprint.HTML(string=rendered_string)
    report = os.path.join(DEST_DIR, OUTPUT_FILENAME)
    html.write_pdf(report, stylesheets=[css])
    print('file is generated...')


if __name__ == '__main__':
    start()
```

Run `python pdf_ticket.pdf` will generate a PDF file directly.

## My thought
Render HTML content into Web page really helps. It is just like web programming, you can use webtool debug on your CSS and layouts, it is easily to check the margins, paddings and other details and it saves me a lot of time. Once I was happy about the web design, I will generate the PDF report and check how it really looks like.

Hope it helps, happy coding!