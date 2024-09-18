import tkinter as tk
from tkinter import ttk
from PIL import Image, ImageTk
from newspaper import Article
from sumy.parsers.plaintext import PlaintextParser
from sumy.nlp.tokenizers import Tokenizer
from sumy.summarizers.lex_rank import LexRankSummarizer
from textblob import TextBlob
import requests
from io import BytesIO

def summarize_article():
    url = entry_url.get()
    try:
        # Fetch the article
        status_var.set("Fetching article...")
        root.update()
        article = Article(url)
        article.download()
        article.parse()

        # Get article details
        title = article.title
        author = ', '.join(article.authors)
        publisher = article.source_url

        # Summarize the article using Sumy
        status_var.set("Summarizing...")
        root.update()
        parser = PlaintextParser.from_string(article.text, Tokenizer('english'))
        summarizer = LexRankSummarizer()
        summary_sentences = summarizer(parser.document, 3)
        summary = ' '.join(str(sentence) for sentence in summary_sentences)

        # Analyze the sentiment of the article using TextBlob
        status_var.set("Analyzing sentiment...")
        root.update()
        blob = TextBlob(article.text)
        polarity = blob.sentiment.polarity

        # Display results in the GUI
        text_title.config(state=tk.NORMAL)
        text_title.delete('1.0', tk.END)
        text_title.insert(tk.END, title)
        text_title.config(state=tk.DISABLED)

        text_author.config(state=tk.NORMAL)
        text_author.delete('1.0', tk.END)
        text_author.insert(tk.END, author)
        text_author.config(state=tk.DISABLED)

        text_publisher.config(state=tk.NORMAL)
        text_publisher.delete('1.0', tk.END)
        text_publisher.insert(tk.END, publisher)
        text_publisher.config(state=tk.DISABLED)

        text_summary.config(state=tk.NORMAL)
        text_summary.delete('1.0', tk.END)
        text_summary.insert(tk.END, summary)
        text_summary.config(state=tk.DISABLED)

        text_polarity.config(state=tk.NORMAL)
        text_polarity.delete('1.0', tk.END)
        text_polarity.insert(tk.END, polarity)
        text_polarity.config(state=tk.DISABLED)

        status_var.set("Article summarized successfully.")

    except Exception as e:
        status_var.set(f"Error: {e}")
        root.update()

def clear_fields():
    entry_url.delete(0, tk.END)
    text_title.config(state=tk.NORMAL)
    text_title.delete('1.0', tk.END)
    text_title.config(state=tk.DISABLED)
    text_author.config(state=tk.NORMAL)
    text_author.delete('1.0', tk.END)
    text_author.config(state=tk.DISABLED)
    text_publisher.config(state=tk.NORMAL)
    text_publisher.delete('1.0', tk.END)
    text_publisher.config(state=tk.DISABLED)
    text_summary.config(state=tk.NORMAL)
    text_summary.delete('1.0', tk.END)
    text_summary.config(state=tk.DISABLED)
    text_polarity.config(state=tk.NORMAL)
    text_polarity.delete('1.0', tk.END)
    text_polarity.config(state=tk.DISABLED)
    status_var.set("Fields cleared.")

# Create the GUI window
root = tk.Tk()
root.title("Article Summarizer")

# Set background image
logo_image = Image.open("fsDOG.jpg")
background_photo = ImageTk.PhotoImage(logo_image)
background_label = tk.Label(root, image=background_photo)
background_label.place(relwidth=1, relheight=1)

# Create input field for the URL
label_url = tk.Label(root, text="Enter Article URL:", bg="lightgray")
label_url.grid(row=0, column=0, padx=10, pady=5, sticky="w")
entry_url = tk.Entry(root, width=50)
entry_url.grid(row=0, column=1, padx=10, pady=5)

# Button to trigger article summarization
summarize_button = tk.Button(root, text="Summarize Article", command=summarize_article, bg="lightgray")
summarize_button.grid(row=0, column=2, padx=10, pady=5)

# Clear button
clear_button = tk.Button(root, text="Clear Fields", command=clear_fields, bg="lightgray")
clear_button.grid(row=0, column=3, padx=10, pady=5)

# Display areas for the article details
label_title = tk.Label(root, text="Title:", bg="lightgray")
label_title.grid(row=1, column=0, padx=10, pady=5, sticky="w")
text_title = tk.Text(root, height=1, width=80)
text_title.grid(row=1, column=1, columnspan=3, padx=10, pady=5)
text_title.config(state=tk.DISABLED)

label_author = tk.Label(root, text="Author(s):", bg="lightgray")
label_author.grid(row=2, column=0, padx=10, pady=5, sticky="w")
text_author = tk.Text(root, height=1, width=80)
text_author.grid(row=2, column=1, columnspan=3, padx=10, pady=5)
text_author.config(state=tk.DISABLED)

label_publisher = tk.Label(root, text="Publisher:", bg="lightgray")
label_publisher.grid(row=3, column=0, padx=10, pady=5, sticky="w")
text_publisher = tk.Text(root, height=1, width=80)
text_publisher.grid(row=3, column=1, columnspan=3, padx=10, pady=5)
text_publisher.config(state=tk.DISABLED)

label_summary = tk.Label(root, text="Summary:", bg="lightgray")
label_summary.grid(row=4, column=0, padx=10, pady=5, sticky="w")
text_summary = tk.Text(root, height=10, width=80)
text_summary.grid(row=4, column=1, columnspan=3, padx=10, pady=5)
text_summary.config(state=tk.DISABLED)

label_polarity = tk.Label(root, text="Polarity:", bg="lightgray")
label_polarity.grid(row=5, column=0, padx=10, pady=5, sticky="w")
text_polarity = tk.Text(root, height=1, width=80)
text_polarity.grid(row=5, column=1, columnspan=3, padx=10, pady=5)
text_polarity.config(state=tk.DISABLED)

# Status bar
status_var = tk.StringVar()
status_bar = tk.Label(root, textvariable=status_var, bd=1, relief=tk.SUNKEN, anchor=tk.W)
status_bar.grid(row=6, column=0, columnspan=4, sticky="we")

root.mainloop()
