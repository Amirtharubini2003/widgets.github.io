# widgets.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Custom Related List</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 10px; background: #f9f9f9; }
        .card-container { display: flex; flex-direction: column; gap: 8px; }
        .item-card { background: #fff; padding: 12px; border-radius: 6px; box-shadow: 0 2px 4px rgba(0,0,0,0.05); display: flex; justify-content: space-between; align-items: center; border-left: 4px solid #0066cc; }
        .item-title { font-weight: bold; color: #333; }
        .item-sub { font-size: 12px; color: #666; }
        .no-data { text-align: center; color: #888; padding: 20px; }
    </style>
</head>
<body>
    <div id="list-container" class="card-container">
        <div class="no-data">Loading related items...</div>
    </div>

    <script src="https://live.zwidgets.com/js-sdk/1.1/ZohoEmbededAppSDK.min.js"></script>
    <script>
        ZOHO.embeddedApp.on("PageLoad", function(data) {
            var dealId = data.EntityId;
            
            // Fetch related contacts (Change 'Contacts' to 'Products' if needed)
            ZOHO.CRM.RelatedList.get({Entity:"Deals",RecordID:dealId,RelatedList:"Contacts"})
            .then(function(response){
                var container = document.getElementById("list-container");
                container.innerHTML = "";
                
                let records = response.data;
                if(!records || records.length === 0) {
                    container.innerHTML = '<div class="no-data">No related records found.</div>';
                    return;
                }

                records.forEach(function(item) {
                    let card = document.createElement("div");
                    card.className = "item-card";
                    card.innerHTML = `
                        <div>
                            <div class="item-title">${item.Full_Name || item.Name || "N/A"}</div>
                            <div class="item-sub">${item.Email || item.Product_Code || ""}</div>
                        </div>
                    `;
                    container.appendChild(card);
                });
            })
            .catch(function(err) {
                console.error(err);
                document.getElementById("list-container").innerHTML = '<div class="no-data">Error loading related data.</div>';
            });
        });

        ZOHO.embeddedApp.init();
    </script>
</body>
</html>

