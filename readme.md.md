In the index.js file we will write the AJAX call to save the entry. 
The steps are as follows:
1. Entry is saved when the Save This Entry button is clicked. 
So write using the click() method of jQuery.
2. As soon as this button is clicked, thedate, text and emotions are saved in the save_data variable.
3. Now create an AJAX call. Define the type of request sent, url of the API.
4. This call will send the data in JSON format.
5. On successful saving of entry, the window is reloaded for entering the text again.
6. If any error occurs, an error message will be displayed.



$("#save_button").click(function () {
        save_data = {
            "date": display_date,
            "text": $("#text").val(),
            "emotion": predicted_emotion
        }
        $.ajax({
            type: 'POST',
            url: "/save-entry",
            data: JSON.stringify(save_data),
            dataType: "json",
            contentType: 'application/json',
            success: function () {
                alert("Your entry has been saved successfully!")
                window.location.reload()
            },
            error: function (result) {
                alert(result.responseJSON.message)
            }
        })   
})



Open app.py for writing the API.
1. Use the route function for redirecting
the Flask to save the entry.
2. Inside the API we’ll be first saving the
data (date, text and emotions) in
different variables.
3. All the entries are stored in a
comma-separated manner using the
variable entry.
4. The file_handler variable is used to
open the CSV file for saving the entry,
5. The open() method is used to open the
CSV file. ‘a’ stands for the append
method so that data can be appended in
the file using the write() method.
6. The entries are then appended and
saved in the file.
7. On success, the data is sent in JSON
format.








# Save entry
@app.route("/save-entry", methods=["POST"])
def save_entry():

    # Get Date, Predicted Emotion & Text Enter by the user to save the entry
    date = request.json.get("date")           
    emotion = request.json.get("emotion")
    save_text = request.json.get("text")

    save_text = save_text.replace("\n", " ")

    # CSV Entry
    entry = f'"{date}","{save_text}","{emotion}"\n'  

    with open("./static/assets/data_files/data_entry.csv", "a") as f:
        f.write(entry)
    return jsonify("Success")