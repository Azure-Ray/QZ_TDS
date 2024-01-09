// ... 原有的JavaScript代码 ...

document.getElementById('save-button').addEventListener('click', function() {
    var updatedData = [];
    var rows = document.getElementById('data-table').rows;
    for (var i = 1; i < rows.length; i++) {
        var row = rows[i];
        updatedData.push({
            crNumber: row.cells[0].innerText,
            crOwner: row.cells[1].innerText,
            imageUrl: row.cells[2].querySelector('input').value,
            dast: row.cells[3].querySelector('input').value,
            sast: row.cells[4].querySelector('input').value
        });
    }

    updatedData.forEach(data => {
        saveData(data);
    });
});

function saveData(data) {
    fetch('http://localhost:8080/aaas', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
    }).then(response => {
        if (response.ok) {
            console.log('Data saved successfully.');
        } else {
            console.error('Failed to save data.');
        }
    }).catch(error => {
        console.error('Error saving data:', error);
    });
}
