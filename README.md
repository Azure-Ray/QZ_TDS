<!-- HTML界面 -->
<div style="font-family: Arial, sans-serif;">
    <h2>Change Request Details</h2>
    <table id="data-table" style="width: 100%; border-collapse: collapse;">
        <!-- 表格头部和数据将通过JavaScript动态插入 -->
    </table>
    <button id="save-button" style="margin-top: 20px;">Save Changes</button>
</div>

<style>
    #data-table, #data-table th, #data-table td {
        border: 1px solid black;
        border-collapse: collapse;
        text-align: left;
        padding: 8px;
    }
</style>

<script>
// JavaScript代码
var userElement = document.getElementById('user');
var apiUrl = 'http://www.test01.com/api';
if (userElement && userElement.value) {
    apiUrl += '?user=' + encodeURIComponent(userElement.value);
}

function fetchDataAndUpdateTable() {
    fetch(apiUrl, { method: 'GET' })
        .then(response => response.json())
        .then(data => displayDataInTable(data))
        .catch(error => {
            console.error('Error fetching data:', error);
            useMockData(); // 当API调用失败时使用测试数据
        });
}

function useMockData() {
    var mockApiResponse = [
        { crNumber: '1001', crOwner: 'Dave', imageUrl: 'http://example.com/image1.jpg', dast: 'DAST1', sast: 'SAST1' },
        { crNumber: '1002', crOwner: 'Sarah', imageUrl: 'http://example.com/image2.jpg', dast: 'DAST2', sast: 'SAST2' },
        { crNumber: '1003', crOwner: 'John', imageUrl: 'http://example.com/image3.jpg', dast: 'DAST3', sast: 'SAST3' }
    ];
    displayDataInTable(mockApiResponse);
}

function displayDataInTable(data) {
    var table = document.getElementById('data-table');
    table.innerHTML = ''; // 清空表格

    // 创建表头
    var header = table.createTHead();
    var headerRow = header.insertRow(0);
    ['CR#', 'CR Owner', 'Image URL', 'DAST', 'SAST'].forEach((title, index) => {
        var cell = headerRow.insertCell(index);
        cell.innerHTML = title;
        cell.style.border = '1px solid black';
    });

    // 填充数据
    data.forEach(item => {
        var row = table.insertRow(-1);
        row.insertCell(0).innerHTML = item.crNumber;
        row.insertCell(1).innerHTML = item.crOwner;

        var imageUrlCell = row.insertCell(2);
        imageUrlCell.innerHTML = '<input type="text" value="' + item.imageUrl + '" />';
        imageUrlCell.style.border = '1px solid black';

        var dastCell = row.insertCell(3);
        dastCell.innerHTML = '<input type="text" value="' + item.dast + '" />';
        dastCell.style.border = '1px solid black';

        var sastCell = row.insertCell(4);
        sastCell.innerHTML = '<input type="text" value="' + item.sast + '" />';
        sastCell.style.border = '1px solid black';
    });
}

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

    // 这里可以调用保存数据的API
    console.log('Updated Data:', updatedData);
});

// 页面加载时立即调用API
fetchDataAndUpdateTable();
</script>
