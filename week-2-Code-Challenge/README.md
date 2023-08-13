--- WEEK 2 PHASE 1 2ND CODE CHALLENGE ---

In this challenge, I have created a project directory, a new GitHub repository and  a db.json file and use this dataLinks to an external site for the server DB.

I ran this command to get the backend started: json-server --watch db.json

Tested the server by visiting this route in the browser: http://localhost:3000/characters and it worked perfectly

I have also added my TM (Mr. Prince Israel) as a contributor to the project

----Core Deliverables----

In this challenge, a user can:

1. See a list of all animal names, which involves making a GET request to the required endpoint to retrieve the character data

2. Click on an animal’s name to see its details i.e image and number of votes. It displays the details of one animal at a time.

3. When viewing an animal’s details, user is able to add the number of votes for each animal. This number of votes should then be displayed together with the animal’s details

The .js file content is shown below:

const container = document.querySelector ('#container')
const listOfAnimals = document.querySelector('#list-names')
const h2 = document.querySelector ('#container h2')
const h6 = document.querySelector ('#container h6')
container.style.cursor = "pointer"

h6.addEventListener ('click', (e) => {
    fetch ('http://localhost:3000/characters')
    .then (Response => Response.json())
    .then (datas => renderNames(datas))


function renderNames (datas) {
    for (const data of datas) {
        const li = document.createElement ('li')
        li.id = data.id; li.classList.add('names')
        li.textContent = data.name
        listOfAnimals.append (li)
    }
    h6.textContent = "Click animal name to view details"
}
})


function renderDetails (datas) {
    listOfAnimals.innerHTML = " "
    h6.innerHTML = " "
    h2.innerHTML = `${datas.name}`; 
    const backButton = document.createElement ('button'); 
    backButton.id = 'back'; backButton.textContent = 'Back'
    const li = document.createElement ('li'); li.append(backButton)
    li.id = datas.id; li.classList.add('names'); li.style.listStyleType = "none"; 
    li.innerHTML = `${datas.name}  <br><br> <img src = ${datas.image} 
    style = "width: 150px"> <br><br> ${datas.votes} votes   `
    const addVotesButton = document.createElement ('button') 
    addVotesButton.id = 'reset'; addVotesButton.textContent = 'Add'
    li.append(addVotesButton)
    listOfAnimals.append(li)
    addVotesButton.addEventListener('click', (e) => {
        fetch (`http://localhost:3000/characters/${datas.id}`, {
            method: 'PATCH',
            headers: {
                'Content-Type': 'application/json',
                Accept: 'application/json'
            },
            body: JSON.stringify ({votes: ++datas.votes})
        })
        .then (Response => Response.json())
        .then (datas => li.innerHTML = `${datas.name} <br><br> <img src = ${datas.image} 
        style = "width: 150px"> <br><br> ${datas.votes} votes   `)
    })
    const resetButton = document.createElement ('button') 
    resetButton.id = 'reset'; resetButton.textContent = 'Reset'
    li.append(resetButton)
    resetButton.addEventListener('click', (e) => {
        fetch (`http://localhost:3000/characters/${datas.id}`, {
            method: 'PATCH',
            headers: {
                'Content-Type': 'application/json',
                Accept: 'application/json'
            },
            body: JSON.stringify ({votes: 0})
        })
        .then (Response => Response.json())
        .then (datas => li.innerHTML = `${datas.name} <br><br> <img src = ${datas.image} 
        style = "width: 150px"> <br><br> ${datas.votes} votes   `)
    })
}

listOfAnimals.addEventListener ('click', (event) => {
    if (event.target.className === 'names') {
        fetch (`http://localhost:3000/characters/${event.target.id}`)
        .then (Response => Response.json())
        .then (datas => renderDetails (datas))
    }
})