fetch('/api/get-date')
            .then(response => response.json())
            .then(data => {
                // 解析 ISO 8601 格式的日期时间字符串
                const date = new Date(data);
                // 格式化日期时间
                const formattedDate = new Intl.DateTimeFormat('default', {
                    year: 'numeric',
                    month: '2-digit',
                    day: '2-digit',
                    hour: '2-digit',
                    minute: '2-digit',
                    second: '2-digit'
                }).format(date);
                setReportDate(formattedDate);
            });
