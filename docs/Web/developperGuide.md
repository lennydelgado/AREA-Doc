# Area Developpement Guide

## Introduction

Welcome to the Area Development Guide! This comprehensive resource is designed to assist developers in understanding and contributing to the development of the Area platform. Whether you are new to the project or an experienced contributor, this guide will provide you with the necessary information to dive into the codebase and enhance the capabilities of Area.

## About Area

The goal of this project is to discover, as a whole, the software platform that you have chosen through the
creation of a business application.

To do this, you must implement a software suite that functions similar to that of IFTTT and/or Zapier.
This software suite will be broken into three parts :

• An application server to implement all the features listed below (see Features)

• A web client to use the application from your browser by querying the application server

• A mobile client to use the application from your phone by querying the application server

## How to Use This Guide

This user guide is organized into sections, each focusing on a specific aspect of Area. Use the following table of contents to navigate to the topics that interest you the most:

- **[Section 1: Prerequisites](#Prerequisites)**

    Learn how to install dependencies and prerequisites for launch Area web site.

- **[Section 2: Getting Started](#getting-started)**

    - [Add a new service](#add-new-service)
    - [Add new action](#add-new-action)

## Prerequisites

Before you begin, make sure you have the following installed:

- [Node.js](https://nodejs.org/)
- [Yarn](https://yarnpkg.com/)

For MacOS use Hombrew:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

brew install node

brew install yarn
```

## Getting Started

If you're new to Area development, follow the steps in the "Getting Started" section to set up your development environment, clone the repository, and get acquainted with our coding conventions.

## Add new service

For add a new service follow this following steps:

1. Add Service in "AREA-WEB/src/app/form-add-action.jsx" file:

```jsx
const services = {
    'Timer': ['Hours check', 'Daily check'],
    'Spotify': ['Is Listening', 'Create Playlist', 'Add to Playlist', 'Delete Song from Playlist', 'Create Recommended'],
    'Discord': ['Send Message'],
    'Email': ['Send Email'],
}
```

Add new service and this actions in this part.

2. For add the new service in "Manages accounts" use this following template in "AREA-WEB/src/app/manageAccount.jsx" file:

```jsx
export const CardAccount = (props) => {
    const [connect, setConnect] = useState(false)

    const handleClick = () => {
        setConnect(!connect)
    }

    const handleDisconnectAndClick = (name) => {
        setConnect(!connect)
        handleDisconnect(name)
    }

    return (
        <div className='mx-auto max-w-sm p-6 bg-white border border-gray-600 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700'>
            <div className='flex items-center mb-4'>
                <div className='w-10 h-10 mb-3 rounded-full overflow-hidden shadow-md'>
                    <img src={process.env.PUBLIC_URL + '/Img/services/' + props.name + '.png'} alt='logo' />
                </div>
                <div className='ml-4'>
                    <h5 className='text-2xl font-semibold tracking-tight text-gray-900 dark:text-white'>
                        {props.name}
                    </h5>
                    {connect ? <p className='text-gray-500'>Connected</p> : <p className='text-gray-500'>Not connected</p>}
                </div>
            </div>

            <p className='my-4 font-normal text-gray-500 dark:text-gray-400'>
                Unlock the potential about connecting <span className='font-bold'>{props.name}</span> with other apps!
            </p>
            {connect ?
                <a onClick={() => handleDisconnectAndClick('')} className='px-4 py-2 mt-2 text-sm font-medium tracking-wide text-red-600 capitalize transition-colors duration-200 transform bg-white border border-red-600 rounded-md hover:bg-red-600 hover:text-white focus:outline-none focus:bg-red-700 focus:text-white'>
                    Disconnect
                </a>
                :
                <a onClick={handleClick} className='px-4 py-2 mt-2 text-sm font-medium tracking-wide text-white capitalize transition-colors duration-200 transform bg-green-600 rounded-md hover:bg-green-700 focus:outline-none focus:bg-green-700'>
                    Connect
                </a>
            }
        </div>
    )
}
```

3. For link front to back, use this following function in "AREA-WEB/src/app/manageAccount.jsx" file:

```jsx
const sendTokenToBack = async (name, token) => {
    console.log('Calling sendtokentoback with spoti')
    axios.post('http://127.0.0.1:3333/services/add', {
        serviceName: name,
        token: token
    }, {
        headers: {
            Authorization: `Bearer ${localStorage.getItem('token')}`
        }
    })
        .then((response) => {
            console.log('sendTokenToBack response:', response)
        })
        .catch((error) => {
            console.log('sendTokenToBack error:', error.message)
        })
}
```

Replace "name" by your service name.

## Add new action

For add new action is verry simple. Simply add the actions you want in the following function in "AREA-WEB/src/app/form-add-action.jsx" file:

```jsx
const services = {
    'Timer': ['Hours check', 'Daily check'],
    'Spotify': ['Is Listening', 'Create Playlist', 'Add to Playlist', 'Delete Song from Playlist', 'Create Recommended'],
    'Discord': ['Send Message'],
    'Email': ['Send Email'],
}
```

For set other services, please see [API Developement Guide](../Api/developperGuide.md)
