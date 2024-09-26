<!-- 
<script>
  let isEuropeanFormat = false;
  let numberInput = 0;
  let displayValue = '';

  // Function to format the number based on the selected format
  function formatNumber(num) {
    if (isNaN(num)) return ''; // Return empty if input is not a valid number
    return isEuropeanFormat
      ? num.toLocaleString('de-DE', { minimumFractionDigits: 0, maximumFractionDigits: 20 }) // European format with decimal support
      : num.toLocaleString('en-US', { minimumFractionDigits: 0, maximumFractionDigits: 20 }); // Metric format with decimal support
  }

  // Update display value based on the format
  function updateDisplayValue() {
    displayValue = formatNumber(numberInput);
  }

  // Toggle the format and update the display
  function toggleFormat() {
    isEuropeanFormat = !isEuropeanFormat;
    updateDisplayValue();
  }

  // Handle input change and update numberInput
  function handleInput(event) {
    let value = event.target.value;
		console.log(value);
    // Remove any existing format characters (commas/dots) for internal storage
    if (isEuropeanFormat) {
      value = value.replace(/\./g, '').replace(',', '.'); // Convert European format to a usable number format
    } else {
      value = value.replace(/,/g, ''); // Remove thousand separators for metric format
    }

    // Check if the value has a trailing decimal point
    const hasTrailingDecimal = isEuropeanFormat
      ? value.endsWith('.')
      : value.endsWith('.');

    // If the value is a valid number and does not have a trailing decimal, update numberInput
    if (!isNaN(value) && !hasTrailingDecimal && value !== '') {
      numberInput = parseFloat(value);
      displayValue = formatNumber(numberInput);
    } else {
      // Preserve the input value if it has an incomplete decimal
      displayValue = event.target.value;
    }

    event.target.value = displayValue; // Set the formatted value back to the input field
  }

  // Prevent non-numeric input except for necessary control keys
  function handleKeyDown(event) {
    const allowedKeys = ['Backspace', 'ArrowLeft', 'ArrowRight', 'Delete', 'Tab'];
    const decimalSeparator = isEuropeanFormat ? ',' : '.';

    // Allow control keys, numbers, and one decimal separator
    if (
      !allowedKeys.includes(event.key) && // Allow navigation and deletion keys
      !/[0-9]/.test(event.key) && // Allow number keys
      !(event.key === decimalSeparator && !event.target.value.includes(decimalSeparator)) // Allow single decimal separator
    ) {
      event.preventDefault(); // Prevent any other keys
    }
  }

  // Initialize display value
  updateDisplayValue();
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }

  .toggle-button {
    margin-bottom: 10px;
    padding: 10px 20px;
    cursor: pointer;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 16px;
  }

  .number-input {
    text-align: center;
    font-size: 24px;
    padding: 10px;
    width: 100%;
    max-width: 400px;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }
</style>

<div class="container">
  <button class="toggle-button" on:click={toggleFormat}>
    {isEuropeanFormat ? 'Switch to Metric Format' : 'Switch to European Format'}
  </button>

  <input
    type="text"
    class="number-input"
    bind:value={displayValue}
    on:input={handleInput}
    on:keydown={handleKeyDown}
    placeholder="Enter a number"
  />
</div>


 -->

<script>
  let isEuropeanFormat = false;
  let numberInput = 0;
  let displayValue = '';

  // Function to format the number based on the selected format
  function formatNumber(num) {
    if (isNaN(num)) return ''; // Return empty if input is not a valid number
    return isEuropeanFormat
      ? num.toLocaleString('de-DE', { minimumFractionDigits: 0, maximumFractionDigits: 20 }) // European format with decimal support
      : num.toLocaleString('en-US', { minimumFractionDigits: 0, maximumFractionDigits: 20 }); // Metric format with decimal support
  }

  // Update display value based on the format
  function updateDisplayValue() {
    displayValue = formatNumber(numberInput);
  }

  // Toggle the format and update the display
  function toggleFormat() {
    isEuropeanFormat = !isEuropeanFormat;
    // Convert current displayValue to the new format
    // let formattedValue = displayValue.replace(/,/g, '').replace(/\./g, isEuropeanFormat ? ',' : '.');
    // numberInput = parseFloat(formattedValue.replace(',', '.')) || 0;
    updateDisplayValue();
  }

			// console.log('hasTrailingDecimal: ', hasTrailingDecimal ,' decimalMatch: ', decimalMatch);
			// console.log('new numberInput', numberInput);
  // Handle input change and update numberInput
  function handleInput(event) {
    let value = event.target.value;

    // Remove any existing format characters for internal storage
    value = isEuropeanFormat ? value.replace(/\./g, '').replace(',', '.') : value.replace(/,/g, '');

    // Check if the value has a trailing decimal point or only zeros after the decimal
    const hasTrailingDecimal = value.endsWith('.') || value.endsWith(',');
    const decimalMatch = isEuropeanFormat
      ? value.match(/^\d*,\d*0+$/) // Match trailing zeros for European format
      : value.match(/^\d*\.\d*0+$/); // Match trailing zeros for Metric format

    // If the value is a valid number or has a trailing decimal, update numberInput, else preserve the input value
    if (!isNaN(parseFloat(value)) && value !== '' && !hasTrailingDecimal && !decimalMatch) {
      numberInput = parseFloat(value) || 0;
      updateDisplayValue(); // Apply formatting
    } else {
      displayValue = event.target.value; // Keep the raw input as-is
    }
  }

  // Prevent non-numeric input except for necessary control keys
  function handleKeyDown(event) {
    const allowedKeys = ['Backspace', 'ArrowLeft', 'ArrowRight', 'Delete', 'Tab'];
    const decimalSeparator = isEuropeanFormat ? ',' : '.';

    // Allow control keys, numbers, and one decimal separator
    if (
      !allowedKeys.includes(event.key) && // Allow navigation and deletion keys
      !/[0-9]/.test(event.key) && // Allow number keys
      !(event.key === decimalSeparator && !event.target.value.includes(decimalSeparator)) // Allow single decimal separator
    ) {
      event.preventDefault(); // Prevent any other keys
    }
  }

  // Initialize display value
  updateDisplayValue();
</script>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }

  .toggle-button {
    margin-bottom: 10px;
    padding: 10px 20px;
    cursor: pointer;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 16px;
  }

  .number-input {
    text-align: center;
    font-size: 24px;
    padding: 10px;
    width: 100%;
    max-width: 400px;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }
</style>

<div class="container">
  <button class="toggle-button" on:click={toggleFormat}>
    {isEuropeanFormat ? 'Switch to Metric Format' : 'Switch to European Format'}
  </button>

  <input
    type="text"
    class="number-input"
    bind:value={displayValue}
    on:input={handleInput}
    on:keydown={handleKeyDown}
    placeholder="Enter a number"
  />
	<p>Input number: {numberInput}</p>
</div>




