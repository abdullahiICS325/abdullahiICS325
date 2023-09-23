<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hardware Store Order</title>
</head>
<body>
    <h1>Hardware Store Order</h1>
    <form id="orderForm">
        <label for="customerName">Customer Name:</label>
        <input type="text" id="customerName" required><br><br>

        <label for="customerAddress">Customer Address:</label>
        <input type="text" id="customerAddress" required><br><br>

        <label for="isSubscribed">Subscribed Member:</label>
        <input type="checkbox" id="isSubscribed"><br><br>

        <label>Select Hardware Items:</label><br>
        <div id="hardwareItems">
            <label for="bulb">Bulb - $2.00 each:</label>
            <input type="number" id="bulb" min="0" value="0"><br>

            <label for="screwdriver">Screwdriver - $5.00 each:</label>
            <input type="number" id="screwdriver" min="0" value="0"><br>

            <label for="hammer">Hammer - $10.00 each:</label>
            <input type="number" id="hammer" min="0" value="0"><br>
        </div>

        <input type="submit" value="Place Order">
    </form>

    <h2>Order Confirmation</h2>
    <p><strong>Customer Name:</strong> <span id="confirmedCustomerName"></span></p>
    <p><strong>Customer Address:</strong> <span id="confirmedCustomerAddress"></span></p>
    <p><strong>Subscription Status:</strong> <span id="confirmedSubscriptionStatus"></span></p>
    <p><strong>Ordered Items:</strong></p>
    <ul id="confirmedOrderedItems"></ul>
    <p><strong>Subtotal:</strong> $<span id="confirmedSubtotal"></span></p>
    <p><strong>Shipping Fee:</strong> $<span id="confirmedShippingFee"></span></p>
    <p><strong>Total Amount Due (including 10% sales tax):</strong> $<span id="confirmedTotalAmountDue"></span></p>

    <script>
        document.getElementById("orderForm").addEventListener("submit", function (e) {
            e.preventDefault();

            // Get customer details
            const customerName = document.getElementById("customerName").value;
            const customerAddress = document.getElementById("customerAddress").value;
            const isSubscribed = document.getElementById("isSubscribed").checked;

            // Get selected hardware items and quantities
            const items = {
                "Bulb": parseInt(document.getElementById("bulb").value),
                "Screwdriver": parseInt(document.getElementById("screwdriver").value),
                "Hammer": parseInt(document.getElementById("hammer").value)
            };

            // Calculate subtotal
            let subtotal = 0;
            for (const item in items) {
                subtotal += items[item];
            }

            // Calculate discounts and shipping fee
            let shippingFee = 0;
            let totalAmountDue = subtotal;

            if (isSubscribed) {
                if (subtotal >= 4 && subtotal <= 9) {
                    totalAmountDue *= 0.95; // 5% discount
                } else if (subtotal >= 10) {
                    totalAmountDue *= 0.90; // 10% discount
                }
            } else {
                if (subtotal > 0 && subtotal < 3) {
                    shippingFee = 8; // $8 shipping fee
                }
            }

            totalAmountDue += shippingFee;
            totalAmountDue += totalAmountDue * 0.10; // 10% sales tax

            // Display confirmation
            document.getElementById("confirmedCustomerName").textContent = customerName;
            document.getElementById("confirmedCustomerAddress").textContent = customerAddress;
            document.getElementById("confirmedSubscriptionStatus").textContent = isSubscribed ? "Subscribed" : "Not Subscribed";

            const confirmedOrderedItems = document.getElementById("confirmedOrderedItems");
            confirmedOrderedItems.innerHTML = "";
            for (const item in items) {
                if (items[item] > 0) {
                    const listItem = document.createElement("li");
                    listItem.textContent = `${item} x ${items[item]}`;
                    confirmedOrderedItems.appendChild(listItem);
                }
            }

            document.getElementById("confirmedSubtotal").textContent = subtotal.toFixed(2);
            document.getElementById("confirmedShippingFee").textContent = shippingFee.toFixed(2);
            document.getElementById("confirmedTotalAmountDue").textContent = totalAmountDue.toFixed(2);
        });
    </script>
</body>
</html>
