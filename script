document.addEventListener('DOMContentLoaded', () => {
    fetchData();
});

async function fetchData() {
    try {
        const response = await fetch('data.json');
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        const data = await response.json();
        populateDashboard(data);
    } catch (error) {
        console.error('Fetch error:', error);
    }
}

function populateDashboard(data) {
   
    const metricsContainer = document.querySelector('.metrics-cards');
    if (data.metrics && metricsContainer) {
        metricsContainer.innerHTML = '';
        data.metrics.forEach(metric => {
            const card = document.createElement('div');
            card.className = 'card';
            card.innerHTML = `<h3>${metric.title}</h3><p>${metric.value}</p>`;
            metricsContainer.appendChild(card);
        });
    }

    
    const tableBody = document.querySelector('.data-table tbody');
    if (data.transactions && tableBody) {
        tableBody.innerHTML = ''; 
        data.transactions.forEach(tx => {
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${tx.id}</td>
                <td>${tx.product}</td>
                <td>${tx.date}</td>
                <td>$${tx.amount.toFixed(2)}</td>
            `;
            tableBody.appendChild(row);
        });
    }

    
    if (data.transactions) {
        renderChart(data.transactions);
    }
}

function renderChart(transactions) {
    const canvas = document.getElementById('myChart');
    if (!canvas) return;
    const ctx = canvas.getContext('2d');

    
    const style = getComputedStyle(document.documentElement);
    
    const chartConfig = {
        backgroundColor: style.getPropertyValue('--chart-bg').trim(),
        borderColor: style.getPropertyValue('--chart-border').trim(),
        borderWidth: parseInt(style.getPropertyValue('--chart-line-width')) || 1,
        borderRadius: parseInt(style.getPropertyValue('--chart-radius')) || 0
    };

    const labels = transactions.map(tx => tx.product);
    const amounts = transactions.map(tx => tx.amount);

    let chartStatus = Chart.getChart("myChart"); 
    if (chartStatus !== undefined) {
        chartStatus.destroy();
    }

    new Chart(ctx, {
        type: 'bar',
        data: {
            labels: labels,
            datasets: [{
                label: 'Ventas por Producto ($)',
                data: amounts,
                backgroundColor: chartConfig.backgroundColor,
                borderColor: chartConfig.borderColor,
                borderWidth: chartConfig.borderWidth,
                borderRadius: chartConfig.borderRadius
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                y: {
                    beginAtZero: true
                }
            }
        }
    });
}
