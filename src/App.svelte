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
    updateDisplayValue(); // Directly update the display based on the toggled format
  }

  // Handle input change and update numberInput
  function handleInput(event) {
		debugger;
    let value = event.target.value;

    // Remove existing format characters for internal storage
    value = isEuropeanFormat ? value.replace(/\./g, '').replace(',', '.') : value.replace(/,/g, '');

    // Check if the value has multiple decimal separators and remove extras
    const decimalSeparator = isEuropeanFormat ? ',' : '.';
    const parts = value.split('.');
    if (parts.length > 2) {
      value = parts[0] + decimalSeparator + parts.slice(1).join('');
    }
		
			console.log('value ', value)
    // Check if the value has a trailing decimal or zeros after the decimal
    const hasTrailingDecimal = value.endsWith('.') || value.endsWith(',');
    // const decimalMatch = isEuropeanFormat ? value.match(/^\d*,\d*0*$/) : value.match(/^\d*\.\d*0*$/);
    const decimalMatch = value.match(/^\d*\.\d*0*$/);
		console.log('parts[1]',parts[1])
		const trailingZeros = (parts[1] || '').match(/0+$/);
		
		
    // If the value is valid, update numberInput and format the display, otherwise keep raw input
    if (!isNaN(parseFloat(value.replace(',', '.'))) || hasTrailingDecimal || decimalMatch) {
      numberInput = parseFloat(value.replace(',', '.')) || 0;
			console.log(formatNumber(numberInput),'hasTrailingDecimal',hasTrailingDecimal,'trailing zeros after dec: ', decimalMatch, "   ", trailingZeros, !formatNumber(numberInput).includes(decimalSeparator))
      displayValue = formatNumber(numberInput) + 
				(formatNumber(numberInput).includes(decimalSeparator) ? '':hasTrailingDecimal || trailingZeros  ? decimalSeparator : '') + 
				(decimalMatch && trailingZeros ? trailingZeros:''); // Keep trailing decimal in display
			console.log('value ', value)
			
    } else {
      displayValue = event.target.value; // Preserve the input as is
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
	<p>Input Number: {numberInput}</p>
</div>

