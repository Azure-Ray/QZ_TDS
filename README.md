<!-- HTML界面 -->
<div style="font-family: Arial, sans-serif;">
    <h2>Change Request Details</h2>
    <table id="data-table" style="width: 100%; border-collapse: collapse;">
        <!-- 表格头部和数据将通过JavaScript动态插入 -->
    </table>
    <button id="save-button" style="margin-top: 20px;">Save Changes</button>
</div>

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
        { crNumber: '1001', crOwner: 'Dave', imageUrl: 'http://example.com/image1.jpg' },
        { crNumber: '1002', crOwner: 'Sarah', imageUrl: 'http://example.com/image2.jpg' },
        { crNumber: '1003', crOwner: 'John', imageUrl: 'http://example.com/image3.jpg' }
    ];
    displayDataInTable(mockApiResponse);
}

function displayDataInTable(data) {
    var table = document.getElementById('data-table');
    table.innerHTML = ''; // 清空表格

    // 创建表头
    var header = table.createTHead();
    var headerRow = header.insertRow(0);
    ['CR#', 'CR Owner', 'Image URL'].forEach((title, index) => {
        headerRow.insertCell(index).innerHTML = title;
    });

    // 填充数据
    data.forEach(item => {
        var row = table.insertRow(-1);
        row.insertCell(0).innerHTML = item.crNumber;
        row.insertCell(1).innerHTML = item.crOwner;
        var imageUrlCell = row.insertCell(2);
        imageUrlCell.innerHTML = '<input type="text" value="' + item.imageUrl + '" />';
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
            imageUrl: row.cells[2].querySelector('input').value
        });
    }

    // 这里可以调用保存数据的API
    console.log('Updated Data:', updatedData);
});

// 页面加载时立即调用API
fetchDataAndUpdateTable();
</script>
