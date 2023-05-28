#react #axios #javascript 

```javascript
const response = await api.get(
    `digitalaccount/${guid_account}/Extract/Export/${extension}?startDate=${startDate}&endDate=${endDate}`,
    {
      responseType: "arraybuffer",
      headers: {
        Accept:
          extension === "pdf" ? "application/pdf" : "application/vnd.ms-excel",
      },
    },
  );
  const blob = new Blob([response.data], {
    type:
      extension === "pdf"
        ? "application/pdf"
        : "application/vnd.ms-excel;charset=utf-8",
  });

  const link = document.createElement("a");
  link.href = window.URL.createObjectURL(blob);
  link.download = `extrato-${startDate}-a-${endDate}.${extension}`;
  link.click();
```