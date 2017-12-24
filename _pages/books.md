---
layout: archive
title: Books
permalink: /books/   
my_array:
  - imageUrl: https://images-na.ssl-images-amazon.com/images/I/51bPR2V9fBL._SX404_BO1,204,203,200_.jpg
    url: https://read.amazon.ca/kp/embed?asin=B001GSTOAM&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_ZW3pAb43TMZ3A
    title: "Clean Code: A Handbook of Agile Software Craftsmanship"
  - imageUrl: https://images-na.ssl-images-amazon.com/images/I/51APYREq6iL.jpg
    url: https://read.amazon.ca/kp/embed?asin=B01NAKSWW1&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_3Q3pAbW93B13H
    title: "Blood, Sweat, and Pixels: The Triumphant, Turbulent Stories Behind How Video Games Are Made"
  - imageUrl: https://images-na.ssl-images-amazon.com/images/I/51C3w4IkIWL.jpg
    url: https://read.amazon.com/kp/embed?asin=B01CO34D12&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_WV1ozbYZ8FTYQ
    title: "Payoff: The Hidden Logic That Shapes Our Motivations"
  - imageUrl: https://images-na.ssl-images-amazon.com/images/I/51ZuUYAopiL.jpg
    url: https://read.amazon.com/kp/embed?asin=B00DQ845EA&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_V21ozbREE04J4=100x20
    title: "The Hard Thing About Hard Things: Building a Business When There Are No Easy Answers"
  - imageUrl: https://images-na.ssl-images-amazon.com/images/I/51uJk41nIrL._SX318_BO1,204,203,200_.jpg
    url: https://read.amazon.com/kp/embed?asin=B00INIXU5I&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_hR5ozb608AKMZ
    title: "How We Got to Now: Six Innovations That Made the Modern World"
  - imageUrl: https://images-na.ssl-images-amazon.com/images/I/51kzUUHhMTL._SX331_BO1,204,203,200_.jpg
    url: http://a.co/8vYvCIm
    title: "Deep Work: Rules for Focused Success in a Distracted World"
  - imageUrl: https://images-na.ssl-images-amazon.com/images/I/41xs4vbcTPL._SX327_BO1,204,203,200_.jpg
    url: https://read.amazon.com/kp/embed?asin=B00FUZQYBO&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cahpzb4FQD45E
    title: "Creativity, Inc.: Overcoming the Unseen Forces That Stand in the Way of True Inspiration"
  - imageUrl: https://images-na.ssl-images-amazon.com/images/I/51ejdeSXFjL.jpg
    url: https://read.amazon.com/kp/embed?asin=B00DGZKQM8&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_-chpzbSVK2EWZ
    title: "Leaders Eat Last: Why Some Teams Pull Together and Others Don't"
  - imageUrl: https://images-na.ssl-images-amazon.com/images/I/41BKx1AxQWL._SX396_BO1,204,203,200_.jpg
    url: https://read.amazon.com/kp/embed?asin=B003GCTQAE&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_9XRAzbRY6MWJQ
    title: "The Pragmatic Programmer: From Journeyman to Master"
  - imageUrl: https://images-na.ssl-images-amazon.com/images/I/41vIhBDtpCL.jpg
    url: https://read.amazon.com/kp/embed?asin=B001BAGWQE&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_rESAzbA0CTYDN
    title: "Traffic: Why We Drive the Way We Do"
---

**Favorite Books**

{% for post in page.my_array %}[<img style="width:auto;height:250px;margin-bottom:10px" src="{{post.imageUrl}}">]({{post.url}}) {% endfor %}
