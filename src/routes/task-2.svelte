<html lang="en">
	<head>
		<meta charset="utf-8" />
		<link rel="icon" href="%sveltekit.assets%/favicon.png" />
		<meta name="viewport" content="width=device-width" />
    <title>Task 2</title>
	</head>
	<body>
    <div id="departments"></div>
    <div id="error"></div>
	</body>

  <script>
    let errorHTML = document.querySelector('#error')
    let departmentsHTML = document.querySelector('#departments')
    let departments = []
    let records = ''

    const fetchData = async () => {
      try {
        const response = await fetch('./response.json')
        const result = await response.json()
        departments = result.departments
      } catch (error) {
        errorHTML.innerHTML = 'Something was fucked up'
        console.error(error)
      }
    }

    const createTable = () => {
      departments.forEach(department => {
        if (!department.jobs.length) { return }

        records = ''
        department.jobs.forEach(job => {
          records += `
            <tr>
              <td>${job.title}</td>
              <td>${job.location.name}</td>
              <td><a href="${job.absolute_url}">${job.absolute_url}</td>
            </tr>
          `
        })
        departmentsHTML.innerHTML += `<table><h1>${department.name}</h1>${records}</table>`
      })
    }

    window.addEventListener('DOMContentLoaded', async event => {
      await fetchData()
      await createTable()
    })
  </script>

  <style>
    tr {
      width: 90vw;
      grid-template-columns: 2fr 1fr 2fr;
      display: grid;
    }
    @media (min-width: 1280px) {
      tr {
        width: 60vw;
      }
    }
      td, h1 {
        padding: 1rem;
      }
  </style>
</html>