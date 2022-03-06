<script>
	import supabase from '$lib/db';
	

	let timetable = {
	Monday: [],
	Tuesday: [],
	Wednesday: [],
	Thursday: [],
	Friday: [],
  };
	let curDay;
	let curIndex;
	let curName;
	let curPeriod;
	let curStyle;
	function showCurData(day, index, name, period, style) {
	curDay = day
	curIndex = index
	curName = name
	curPeriod = period
	curStyle = style
	}
	function addTimeSlot(day) {
		if (day === 'Monday') {
			timetable.Monday = [...timetable.Monday, { name: '??', period: 1, style: '' }];
		} else if (day === 'Tuesday') {
			timetable.Tuesday = [...timetable.Tuesday, { name: '??', period: 1, style: '' }];
		} else if (day === 'Wednesday') {
			timetable.Wednesday = [...timetable.Wednesday, { name: '??', period: 1, style: '' }];
		} else if (day === 'Thursday') {
			timetable.Thursday = [...timetable.Thursday, { name: '??', period: 1, style: '' }];
		} else if (day === 'Friday') {
			timetable.Friday = [...timetable.Friday, { name: '??', period: 1, style: '' }];
		}
	} 
	
	async function logout() {
		const { error } = await supabase.auth.signOut();

		if (error) alert(error.message); // alert if error
	}

function deleteTimeSlot(day, index) {
	timetable[day].splice(index, 1)
	timetable=timetable
	saveEntry();
}
function setTimeSlot(day,index,newName,newPeriod,newStyle){
	  timetable [day] [index] . name = newName ;
	  timetable [day] [index] . period = newPeriod ;
	  timetable [day] [index] . style = newStyle ;
	  saveEntry();
}

// Upsert entry
async function saveEntry() {
  const { error } = await supabase.from("studentEntries").upsert(
    {
      user_id: supabase.auth.user().id,
      timetable: timetable,
    },
    { onConflict: "user_id" }
  );
  if (error) alert(error.message);
}
// Get entries
async function getEntries() {
  const { data, error } = await supabase.from("studentEntries").select();
  if (error) alert(error.message);

  if (data != "") {
    timetable = data[0].timetable;
  }
}

getEntries();
</script>

<div class="container mt-5">
	<!-- svelte-ignore a11y-missing-content -->
	<h1>My Dashboard</h1>
	<caption>School Timetable</caption>
	<table class="table text-center table-bordered table-striped caption-top">
		<thead>
			<tr class="table-dark">
				<th scope="col">#</th>
				<th scope="col">1</th>
				<th scope="col">2</th>
				<th scope="col">3</th>
				<th scope="col">4</th>
				<th scope="col">-</th>
				<th scope="col">5</th>
				<th scope="col">6</th>
				<th scope="col">7</th>
				<th scope="col">8</th>
				<th scope="col">9</th>
				<th scope="col">10</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<th class="table-dark" scope="row">MON</th>
				{#each timetable.Monday as timeSlot, index}
					<td colspan={timeSlot.period} class={timeSlot.style}>
						<button type="button" class="btn" data-bs-toggle="modal" data-bs-target="#editTimeSlot"
						on:click={() =>showCurData('Monday', index, timeSlot.name, timeSlot.period, timeSlot.style)}
							>{timeSlot.name}
						</button>
					</td>
				{/each}
				<td>
					<button on:click={() => addTimeSlot('Monday')} class="btn">+</button>
				</td>
			</tr>
			<tr>
				<th class="table-dark" scope="row">TUE</th>
				{#each timetable.Tuesday as timeSlot, index}
					<td colspan={timeSlot.period} class={timeSlot.style}>
						<button type="button" class="btn" data-bs-toggle="modal" data-bs-target="#editTimeSlot"
						on:click={() =>showCurData('Tuesday', index, timeSlot.name, timeSlot.period, timeSlot.style)}
							>{timeSlot.name}
						</button>
					</td>
				{/each}
				<td>
					<button on:click={() => addTimeSlot('Tuesday')} class="btn">+</button>
				</td>
			</tr>
			<tr>
				<th class="table-dark" scope="row">WED</th>
				{#each timetable.Wednesday as timeSlot, index}
					<td colspan={timeSlot.period} class={timeSlot.style}>
						<button type="button" class="btn" data-bs-toggle="modal" data-bs-target="#editTimeSlot"
						on:click={() =>showCurData('Wednesday', index, timeSlot.name, timeSlot.period, timeSlot.style)}
							>{timeSlot.name}
						</button>
					</td>
				{/each}
				<td>
					<button on:click={() => addTimeSlot('Wednesday')} class="btn">+</button>
				</td>
			</tr>
			<tr>
				<th class="table-dark" scope="row">THU</th>
				{#each timetable.Thursday as timeSlot, index}
					<td colspan={timeSlot.period} class={timeSlot.style}>
						<button type="button" class="btn" data-bs-toggle="modal" data-bs-target="#editTimeSlot"
						on:click={() =>showCurData('Thursday', index, timeSlot.name, timeSlot.period, timeSlot.style)}
							>{timeSlot.name}
						</button>
					</td>
				{/each}
				<td>
					<button on:click={() => addTimeSlot('Thursday')} class="btn">+</button>
				</td>
			</tr>
			<tr>
				<th class="table-dark" scope="row">FRI</th>
				{#each timetable.Friday as timeSlot, index}
					<td colspan={timeSlot.period} class={timeSlot.style}>
						<button type="button" class="btn" data-bs-toggle="modal" data-bs-target="#editTimeSlot"
						on:click={() =>showCurData('Friday', index, timeSlot.name, timeSlot.period, timeSlot.style)}
							>{timeSlot.name}
						</button>
					</td>
				{/each}
				<td>
					<button on:click={() => addTimeSlot('Friday')} class="btn">+</button>
				</td>
			</tr>
		</tbody>
	</table>
</div>

<!-- Modal -->
<div
	class="modal fade"
	id="editTimeSlot"
	tabindex="-1"
	aria-labelledby="exampleModalLabel"
	aria-hidden="true"
>
	<div class="modal-dialog">
		<div class="modal-content">
			<div class="modal-header">
				<h5 class="modal-title" id="exampleModalLabel">Edit Time Slot</h5>
				<button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" />
			</div>
			<div class="modal-body">
				<div class="input-group mb-3">
					<span class="input-group-text" id="basic-addon1">Name</span>
					<input type="text" class="form-control" bind:value= {curName}/>
				</div>
				<div class="input-group mb-3">
					<span class="input-group-text" id="basic-addon1">Period </span>
					<input type="number" class="form-control" bind:value= {curPeriod}/>
				</div>
				<div class="input-group mb-3">
					<label class="input-group-text" for="styleSelect">style</label>
					<select class="form-select" id="styleSelect" bind:value= {curStyle}>
						<option value="">Default</option>
						<option value="table-primary">Blue</option>
						<option value="table-success">Green</option>
						<option value="table-danger">Red</option>
						<option value="table-warning">Yellow</option>
						<option value="table-secondary">Grey</option>
					</select>
				</div>
			</div>
			<div class="modal-footer">
				<button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
				<button type="button" on:click={()=> deleteTimeSlot(curDay, curIndex)} class="btn btn-danger" data-bs-dismiss="modal">Delete</button>
				<button type="button" on:click={()=> setTimeSlot(curDay,curIndex,curName,curPeriod,curStyle)} class="btn btn-primary" data-bs-dismiss="modal">Save changes</button>
			</div>
		</div>
	</div>
</div>

<div class="mt-5 text-center">
	<button class="btn btn-dark" on:click={logout}>Logout</button>
</div>
