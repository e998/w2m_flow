<script>
  import Switch from './lib/Switch.svelte'
  import flowLogo from './assets/flowlogo.png'
	let sliderValue;
</script>

<main>
  
  <div>
    <a href="/App.svelte" target="_blank" rel="noreferrer">
      <img src={flowLogo} class="logo" alt="Flow Logo"/>
    </a>
  </div>

  <!-- CALENDAR -->
  <h1>Conflict Planner</h1>
  <p>
    Input the times that you are <span style="font-weight: bold; text-decoration: underline;">unavailable</span>.
    <br>
    Edit how flexible your conflicts are using the dropdown menu below.
  </p>
  <p>
    <!--
      Dropdown question to input level of flexibility. 
      Default is set to Flexible, but we would change this in a future prototype.
      A future prototype would only include the "Completely Inflexible" and "Somewhat Flexible" options
    -->
    <label for="eventColor">Flexibility Level:</label>
      <select id="eventColor">
        <option value="#bfbfbf">Flexible &#40;Light Gray&#41;</option>
        <option value="#6e6e6e">Somewhat Flexible &#40;Gray&#41;</option>
        <option value="#2e2e2e">Completely Inflexible &#40;Dark Gray&#41;</option>
      </select>
      
  </p>

  <br>
  <!--
    Toggle that will be used to switch between Unavailability and Availability
    Not currently working, but would in a future prototype
  -->
  <Switch bind:value={sliderValue} label="Show {sliderValue}" fontSize={18} design="slider"/>
  <br>
  <main class="row">
    
  
    <div id="ec" class="col"></div>
  </main>

  <script type="text/javascript">

    //Calendar that is shown on the prottype
    const ec = new EventCalendar(document.getElementById('ec'), {
      view: 'timeGridWeek',
      headerToolbar: {
        start: 'prev,next today',
        center: 'title',
        end: 'dayGridMonth,timeGridWeek,timeGridDay,listWeek'
      },
      scrollTime: '09:00:00',
      events: createEvents(),
      views: {
        timeGridWeek: {pointer: true},
        resourceTimeGridWeek: {pointer: true}
      },
      dayMaxEvents: true,
      nowIndicator: true,
      selectable: true,
      select: function(info) { // Event handler for date range selection; Help from Chat GPT
        const title = "1"
        if (title) { 
          const colorDropdown = document.getElementById('eventColor'); // Get the dropdown element
          const color = colorDropdown.value;
          ec.addEvent({ // Add the new event to the calendar
            start: info.startStr,
            end: info.endStr,
            color: color || "#bfbfbf"
          });
        }
        ec.unselect(); // Clear the current selection
      }
    });
  
    //Used to set the days of the calendar
    function createEvents() {
      let days = [];
      for (let i = 0; i < 7; ++i) {
        let day = new Date();
        let diff = i - day.getDay();
        day.setDate(day.getDate() + diff);
        days[i] = day.getFullYear() + "-" + _pad(day.getMonth()+1) + "-" + _pad(day.getDate());
      }
      return [];
    }
  
    function _pad(num) {
      let norm = Math.floor(Math.abs(num));
      return (norm < 10 ? '0' : '') + norm;
    }

    
  </script>
  
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  
</style>
