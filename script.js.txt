// Function to fetch prices from API
async function fetchPrices() {
    let product = document.getElementById("searchBox").value;
    if (!product) {
        alert("Please enter a product name");
        return;
    }

    const url = `https://your-api-url.com?query=${product}`; // Replace with your API URL
    const options = {
        method: "GET",
        headers: {
            "X-RapidAPI-Key": "your-api-key", // Replace with your API Key
            "X-RapidAPI-Host": "api-host"
        }
    };

    try {
        const response = await fetch(url, options);
        const data = await response.json();
        updateTable(data); // Step 5: Display Data in Table
    } catch (error) {
        console.error("Error fetching data:", error);
    }
}

// Step 5: Function to Display Data in Table
function updateTable(data) {
    let table = document.getElementById("priceTable");

    // Clear previous results
    table.innerHTML = `
        <tr>
            <th>Store</th>
            <th>Price</th>
            <th>Sentiment</th>
        </tr>
    `;

    data.forEach(item => {
        let sentiment = analyzeSentiment(item.review); // Step 6: Sentiment Analysis
        let row = `<tr>
            <td>${item.store}</td>
            <td>$${item.price}</td>
            <td>${sentiment}</td>
        </tr>`;
        table.innerHTML += row;
    });
}

// Step 6: Function to Perform Sentiment Analysis
function analyzeSentiment(review) {
    const sentiment = new Sentiment(); // Sentiment.js Library
    const result = sentiment.analyze(review);
    return result.score > 0 ? "Positive" : result.score < 0 ? "Negative" : "Neutral";
}
