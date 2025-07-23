# What is a REST API?
REST = REpresentational State Transfer

A RESTful API is an architecture style where we use HTTP methods (like GET, POST, PUT, DELETE) to interact with data stored on the server — like creating, reading, updating, or deleting ("CRUD") resources.
```
fetch('link') //get
router.get('link',(req,res)=>{

})


fetch(`link${variable}`,{method:'DELETE'}) //delete 
router.delete('link:variable',(req,res)=>{
    const variable=req.params.variable
})



fetch('link',{ //post
    method:'POST',
    headers:{
        'Content-Type':'application/json'
    },
    body:JSON.stringify({key:value}) //passing object
})
```
# Tag writing not coding but remeber this point
- if we dont pass something same as img tag where we cant write anything
```
 <img src="" alt="" />
```
``` 
    <Route path='/' element={<Home />} />
```
- if we pass something same as div tag wehere we write someting
 ```
 <h1></h1>
 ```
```
    <Route path='/admin' element={<Admin />} >
        <Route path='dashboard' element={<Dashboard />} />
    </Route>
    
   
```
# MAP
>use this
```
{x.map(item,index)=>(
    <p key={index}>{item.title}</p>
)} 
```
>avoid return in map
```
{x.map(item,index)=>{
    return(<>
   <p key={index}>{item.title}</p> 
    </>)
}
}
```

# Nested Routing using browserrouter
> here inside admin page we inside route put other routes without / 
- localhost:2030/admin/manage-blog  we dont add admin/ but it is added automatically we just use manage-blog inside route of admin    without closing it 
- we use navlink to match the url navigation component made by user
- we use <Outlet/> to display the result in user component 
```
    <BrowserRouter>
      <Routes>
          <Route path='/admin' element={<Admin />} >
              <Route path='dashboard' element={<Dashboard />} />
              <Route path='manage-blog' element={<ManageBlog />} />
              <Route path='manage-gallery' element={<ManageGallery />} />
              <Route path='manage-project' element={<ManageProject />} />
              <Route path='setting' element={<Setting />} />
          </Route>
      </Routes>
    </BrowserRouter>
```

# Example - Nested Routing
```
import { useState } from "react"
import { MdOutlineClose, MdOutlineMenu } from "react-icons/md";
import { BrowserRouter, NavLink, Outlet } from "react-router-dom";
export default function Admin() {

    const [isMenubar, setMenubar] = useState(true)

    const adminMenu = [
        { title: 'Dashboard', link: 'dashboard' },
        { title: 'Manage Gallery', link: 'manage-gallery' },
        { title: 'Manage Project', link: 'manage-project' },
        { title: 'Manage Blog', link: 'manage-blog' },
        { title: 'setting', link: 'setting' },


    ]
    return (<>
        <section className="w-full  h-[100vh] border-2 border-red-400">
            <div onClick={() => setMenubar(!isMenubar)} className="relative top-0 flex items-center bg-primary  h-[40px] px-10 ">{<MdOutlineMenu className="text-text-primary text-3xl " />}</div>
            <div className="relative flex w-full h-full">
                {/* admin navabar */}
                {
                    isMenubar && (
                        <div className=" absolute md:static bg-primary text-text-primary  w-40 h-full flex flex-col justify-evenly items-center">
                            <h1 className="text-xl font-semibold tracking-[1px]">Admin Panel</h1>
                            <ul className=" text-center capitalize  h-[80%]  flex flex-col justify-evenly">
                                {
                                    adminMenu.map((item, index) => (
                                        <li key={index} ><NavLink to={item.link} >{item.title}</NavLink></li>
                                    ))
                                }
                            </ul>
                        </div>
                    )
                }
                {/* admin pages */}
                <div className="border-2  border-black h-full grow  bg-[pink] p-10  ">
                    <div className="border-2 h-full rounded-xl overflow-hidden ">
                        <div className="bg-primary h-12 w-full "></div>
                        <div className="bg-yellow-300 h-full">
                            {/* link to pe open here path is given above */}
                            <Outlet />                                            {/*we display the link here in outlet tag*/}
                        </div>
                    </div>
                </div>
            </div>

        </section>
    </>)
}
``` 
# Public And Private Layout For Client And Admin
- make public layout with navbar and footer because qwe want to dispaly it in every page
- make private layout so that public and private route wont have navbar and footer 
- using nested routing we give path with / inside route where public or private layout is rendered as element look below 
```
import Navbar from './components/Navbar.jsx'
import Footer from './components/Footer.jsx'
import Home from './pages/Home.jsx'
import Blog from './pages/Blog.jsx'
import Gallery from './pages/Gallery.jsx'
import Process from './pages/Process.jsx'
import Project from './pages/Project.jsx'
import Admin from './admin/Admin.jsx'

import Dashboard from './admin/Dashboard.jsx'
import ManageBlog from './admin/ManageBlog.jsx'
import ManageGallery from './admin/ManageGallery.jsx'
import ManageProject from './admin/ManageProject.jsx'
import Setting from './admin/Setting.jsx'

import { BrowserRouter, Route, Routes,Outlet } from 'react-router-dom'

function PublicLayout() {
  return (<>
    <Navbar />
    <Outlet />
    <Footer />
  </>)

}
function AdminLayout() {
  return (<>
    <Admin />
  </>)

}
export default function App() {

  return (<>
    <BrowserRouter>

      <Routes>
        <Route path='/admin' element={<AdminLayout />} >
          <Route path='' element={<Dashboard />} />
          <Route path='manage-blog' element={<ManageBlog />} />
          <Route path='manage-gallery' element={<ManageGallery />} />
          <Route path='manage-project' element={<ManageProject />} />
          <Route path='setting' element={<Setting />} />
        </Route>

        <Route path='/' element={<PublicLayout />}>
          <Route path='' element={<Home />} />
          <Route path='process' element={<Process />} />
          <Route path='projects' element={<Project />} />
          <Route path='blogs' element={<Blog />} />
          <Route path='gallery' element={<Gallery />} />
        </Route>
      </Routes>

    </BrowserRouter>
  </>)
}
```


# Navlink and isActive variable and end variable
- {isactive} is a boolean 
- /admin and /admin/123 is same i.e true for is active without end it consider subroute too
- /admin and /admin/123 is different i.e false if end is used with isactive 

```
✅ Without end

<NavLink to="/about">About</NavLink>
Active on: /about, /about/team, /about/anything

✅ Matches if the current URL starts with /about

✅ With end

<NavLink to="/about" end>About</NavLink>
Active only on: /about

❌ Not active on: /about/team

✅ Matches only if URL is exactly /about
```

```
<NavLink to={item.link} end className={ (({isActive})=>isActive?'bg-yellow-800 p-2 rounded-xl':'')} >{item.title}</NavLink>
```
```
<NavLink
        className={item.title != 'Home' && (({ isActive }) => isActive ? "border-b-2 border-text-primary text-yellow-100" : "hover:text-yellow-100")}
        to={item.link}
        >
        About us
</NavLink>
```

# Routing using browserrouter
> browser route is used to use navlink and route to different page on client withour refreshing the page
- we use navlink to match the url in navigation component made by user
```
    <BrowserRouter>
      <Routes>  
          <Route path='/' element={<Home />} />
          <Route path='/process' element={<Process />} />
          <Route path='/projects' element={<Project />} />
          <Route path='/blogs' element={<Blog />} />
          <Route path='/gallery' element={<Gallery />} />
      </Routes>
    </BrowserRouter>
```

# Responsive Navbar
- render at mobile screen first and hide it on md on mobile screen maintain usestate to open menubar and close menu bar 
- again render at md for desktop and hide it at mobile
> we are using navlink from react-router-dom it help to navigate to different page and provide active class to tell user on which page they are by styling {isactive} in braces we put else everyone get active and get color 

## Example-Responsive Navbar With Routing Code Logic Complete
```
## App.jsx

import Navbar from './components/Navbar.jsx'
import Footer from './components/Footer.jsx'
import Home from './pages/Home.jsx'
import Blog from './pages/Blog.jsx'
import Gallery from './pages/Gallery.jsx'
import Process from './pages/Process.jsx'
import Project from './pages/Project.jsx'
import { BrowserRouter, Route, Routes } from 'react-router-dom'
export default function App() {

return (<>
  <BrowserRouter>
    <Navbar />
    <Routes>
      <Route path='/' element={<Home />} />
      <Route path='/process' element= {<Process />}/>
      <Route path='/projects' element= {<Project/>} />
      <Route path='/blogs' element= {<Blog />}/>
      <Route path='/gallery' element= {<Gallery/>  }/>
    </Routes>
    <Footer />
  </BrowserRouter>
</>)
}
```

```
## Navbar.jsx
import { MdOutlineClose, MdOutlineMenu } from "react-icons/md"; {/*this is menubar and close menu bar icon in svg format*/}
import { useState } from 'react';
import { NavLink } from "react-router-dom";
export default function Navbar() {
    const [isMenuClicked, setMenuClicked] = useState(false)
    const handleMenuBar = () => {
        setMenuClicked(!isMenuClicked);
    }
    const navData = [
        { title: 'Home', link: '/' },
        { title: 'Process', link: '/process' },
        { title: 'Projects', link: '/projects' },
        { title: 'Gallery', link: '/gallery' },
        { title: 'Blogs', link: '/blogs' },
    ]
    return (<>
        <nav className=" px-10 flex sticky top-0 w-full z-10 bg-primary text-text-primary  justify-between h-12 items-center text-base">
            {/* text color  and bg and px-10 */}
            <h1>Dilip Construction</h1>
            <ul className="hidden md:flex gap-4 ">
                {navData.map((item, index) => (

                    <li key={item.title}>
                        <NavLink
                            className={item.title != 'Home' && (({ isActive }) =>
                                isActive ? "border-b-2 border-text-primary text-yellow-100" : "hover:text-yellow-100")}
                            to={item.link}>
                            {item.title}
                        </NavLink>
                    </li>

                ))}
            </ul>
            <div className="md:hidden" onClick={handleMenuBar}>
                {isMenuClicked ? <MdOutlineClose /> : <MdOutlineMenu />}
            </div>

            {isMenuClicked && (
                <ul className="left-0 absolute top-12 w-full gap-8 py-5 bg-gray-800 text-text-primary backdrop-blur-[7px] flex flex-col items-center">
                    {navData.map((item, index) => {
                        return (<>
                            <li onClick={handleMenuBar} key={index}><NavLink to={item.link}>{item.title}</NavLink></li>
                        </>)
                    })}
                </ul>
            )}
        </nav>
    </>)

}
```



# Form
- button tag is by default submit we can use it we can change its type to make normal click button by type='button' 
- disabled={true} will disable button used to avoid sending multiple submission together by default while one is being sent 
- label tag has htmlfor which match id due to which even we are clicking on text of label input box get active to write with 
   corresponding input matched with label htmlfor id
```
 <form onSubmit={sendMail} action="">
    <label htmlFor="name">Name</label>
    <input placeholder="Enter your Name" type="text" name='name' id='name' onChange={handleChange} />
    <input disabled={true} type="submit" value="Submit Details" />
 </form>
```
# Contact Form With Logic
- we want to send object by converting into json to parse object in backend therfore to store object value in realtime we use usestate
- we take empty object with name and value is empty 
- we make this object as usestate variable
- using handlechange function we set form data
- e.target to get name and value it is reserved keyword
- prev function to set only changed data not whole

```
  const formStructure = { name: '', email: '', phoneNo: '', message: '' }
    const [formData, setFormData] = useState(formStructure)

    const handleChange = (e) => {
        const { name, value } = e.target
        setFormData((prev) => {
            return { ...prev, [name]: value }
        })
    }
```
# Cors
> allow to send frontend data to backend and get data from backend to frontend
```
const cors=require('cors')
app.use(cors()) {/*it is cors() not cors*/}
```
# Data Parsing

> Frontend object → JSON.stringify() → 🔁 → Express.json() → Backend object
```
const data = {
  name: "Abhishek",
  email: "abc@example.com",
  message: "Hello",
};

fetch('/api/contact', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(data),  // 🔁 Convert JS object to JSON string
});

app.use(express.json())

req.body.name      // "Abhishek"
req.body.email     // "abc@example.com"
req.body.message   // "Hello"

while fetching data we use  const data = await response.json(); to transform json to object
```

# fetch data
- try catch err and err.message mostly 
- response.ok to check route give response or not
- error in try catch is err.message due to wrong port 
- error in response is error.text due to invalid route
```
        try {
            const response = await fetch(import.meta.env.VITE_MAIL_LINK, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(formData)
            });

            if (!response.ok) {
                // Handle non-200 errors
                const errorText = await response.text();
                console.error("Server error route error:", errorText);
                alert("Something went wrong on the server.");
                return;
            }

            const data = await response.json();
            console.log("Response:", data);
            alert(data.message || "Mail sent successfully!");
        } catch (error) {
            console.error("Fetch error port error:", error.message);
            alert("Failed to send. Please check the server connection or console for details.");
        }
        finally {
            setSendingStatus(false); // Re-enable button
        }
```
#  Example-Sending Form Data From Frontend To Backend
```
    const [sendingStatus, setSendingStatus] = useState(false)
    const sendMail = async (e) => {
        setSendingStatus(true)
        e.preventDefault();

        try {
            const response = await fetch('http://localhost:2030/mail/sent', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(formData)
            });

            if (!response.ok) {
                // Handle non-200 errors
                const errorText = await response.text();
                console.error("Server error:", errorText);
                alert("Something went wrong on the server.");
                return;
            }

            const data = await response.json();
            console.log("Response:", data);
            alert(data.message || "Mail sent successfully!");
        } catch (error) {
            console.error("Fetch error:", error);
            alert("Failed to send. Please check the server connection or console for details.");
        }
        finally {
            setSendingStatus(false); // Re-enable button
        }
    };


                <form onSubmit={sendMail} className="rounded-2xl px-5 py-5 text-gray-300 bg-primary  w-full md:w-[30%] h-[80%] flex flex-col gap-[12px]" action="">
                    <label htmlFor="name">Name</label>
                    <input placeholder="Enter your Name" type="text" name='name' id='name' className="bg-primary border-[1px] px-3 rounded-lg h-8" onChange={handleChange} />
                    <label htmlFor="email">Email *</label>
                    <input placeholder='Enter your Email' type="email" name="email" id='email' required className="bg-primary border-[1px] px-3 rounded-lg h-8" onChange={handleChange} />
                    <label htmlFor="phoneNo">Phone No. *</label>
                    <input placeholder='Enter Phone No' type="tel" name="phoneNo" id='phoneNo' required className="bg-primary border-[1px] px-3 rounded-lg h-8 " onChange={handleChange} />
                    <label htmlFor="message">Message *</label>
                    <textarea name="message" id="message" placeholder='Message' required className="bg-primary border-[1px] px-3 rounded-lg h-16 resize-none " onChange={handleChange} ></textarea>
                    <button disabled={sendingStatus} className=' text-white ml-auto mr-auto w-[80%] bg-text-primary hover:bg-text-primary/20 border-black/45 border-2 px-7 py-2 rounded-2xl' > {sendingStatus ? "Sending..." : "Submit"}</button>
                </form>
```
# Mail Backend
- err.message
- info.response

```
       const transporter = nodemailer.createTransport({
            service: 'gmail',
            auth: {
                user: 'devmagnets.mail@gmail.com', {/*main account of gamil you used to send message*/}
                pass: process.env.GMAIL_PASSWORD {/*create password in google > app password*/}
            }
        })
        const mailOption = {
            from: 'devmagnets.mail@gmail.com', {/*same as user */}
            to: 'singhabhishek.engineer@gmail.com',{/*you choose to get client email*/}
            replyTo: 'xyz@gmail.com,{/*client email where you want to sent reply*/}
            subject: 'inquiry',
            html: <p>hi<p>
        }
        transporter.sendMail(mailOption, (err, info) => {
            if (err) {
                console.log(err.message)
            }
            else {
                console.log(info.response)
            }
        })
```
#  Example-Mail With Logic Backend
- we use <%= x %> to change its variable i.e x, according to request
- we use ejsrenderFile to get html as template to use any where
>    const htmlContent = await ejs.renderFile('./view/emailTemplate.ejs', {
            name: name, email: email, phoneNo: phoneNo, message: message
        });

```
#emailTemplate.ejs


                    <tr>
                        <td>
                            <strong>Name: </strong> <%= name %> 
                        </td>

                    </tr>
                    <tr>
                        <td style="word-break: break-all;">
                            <strong>Email: </strong><%= email %>
                        </td>

                    </tr>
                    <tr>
                        <td>
                            <strong>Phone: </strong><%= phoneNo %>
                        </td>
                    </tr>
#email.js 

const express = require('express')
const router = express.Router()
const ejs = require('ejs')
const nodemailer = require('nodemailer')

router.post('/sent', async (req, res) => {
    const { name, email, phoneNo, message } = req.body
    try {
        const htmlContent = await ejs.renderFile('./view/emailTemplate.ejs', {
            name: name, email: email, phoneNo: phoneNo, message: message
        });
        const transporter = nodemailer.createTransport({
            service: 'gmail',
            auth: {
                user: 'devmagnets.mail@gmail.com',
                pass: process.env.GMAIL_PASSWORD
            }
        })
        const mailOption = {
            from: 'devmagnets.mail@gmail.com',
            to: 'singhabhishek.engineer@gmail.com',
            replyTo: email,
            subject: 'inquiry',
            html: htmlContent
        }
        transporter.sendMail(mailOption, (err, info) => {
            if (err) {
                console.log(err.message)
                res.send({ message: 'failed' })
            }
            else {
                console.log(info.response)
                res.send({ message: 'sent' })
            }
        })
    } catch (err) {
        console.log(err.message)
        res.send('failed try catch')

    }
})

module.exports = router
```

# Separate data content from jsx file 
- we use export or export default like we do in jsx file it is same
```
#processData.js


const step = {
  title: 'Step Process',
  list: ['Consultation', 'Design'],
};

const extra = {
  title: 'Extra Info',
  list: ['Durability', 'Aesthetics'],
};

export { step, extra }; 
{/* 
export default step 
export {extra}
*}

-----------------------------------------------------------------------
#process.jsx


import { step, extra } from '../pagesData/processData';

console.log(step.title);  // Output: 'Step Process'
console.log(extra.list);  // Output: ['Durability', 'Aesthetics']

```
# Separating Route
- using module.export we export routing to use in index.js
- we first import 
- then using use middleware pass it with path i.e app.use('/user', userRoutes) if user request user send to userRoutes means 
if user sent request to user/contact then we will see contact from submitted look below to understand basically both path routing and index both are concatenated
```
#index.js

const userRoutes = require('./routes/userRoutes'); // importing routes
app.use('/user', userRoutes); // prefix all routes with /user

---------------------------------------------------------------------

#routes/userRoutes.js

router.post('/contact', (req, res) => {
  res.send('Contact Form Submitted');
});

module.exports = router;
```

# Example-Separating Route

```
#routes/userRoutes.js


const express = require('express');
const router = express.Router();

// Example route
router.get('/about', (req, res) => {
  res.send('About Page');
});

router.post('/contact', (req, res) => {
  res.send('Contact Form Submitted');
});

module.exports = router;

-----------------------------------------------------------

#index.js


const express = require('express');
const app = express();
const userRoutes = require('./routes/userRoutes'); // importing routes

app.use('/user', userRoutes); // prefix all routes with /user

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```
# DB Connection
- npm i mongoose 
- download MongoDB Community Server ang get connection link
```
const mongoose = require('mongoose')

    mongoose.connect(process.env.DB_LINK)
    .then(() => {
        console.log('db is connected')
    })
    .catch((err) => {
        console.error(`err: ${err}`)
    })

```
# useNavigate and useLocation
- ✅ useNavigate
Purpose:
Used to navigate to a different route programmatically, usually after a user action like button click or form submission.
- ✅ useLocation
Purpose:
Used to get information about the current route, such as the pathname, query parameters, hash, and custom state passed during navigation.
```
✅ useNavigate


Import:
import { useNavigate } from 'react-router-dom'

How it works:

You call useNavigate() to get the navigate function.

Then you call navigate('/path') to go to a new route.

Example:


const navigate = useNavigate();
navigate('/dashboard'); // goes to /dashboard
You can also go back:


navigate(-1); // same as browser back
Use cases:

Button clicks

Form submissions

Redirects after login or logout
```
```

✅ useLocation


Import:
import { useLocation } from 'react-router-dom'

How it works:

You call useLocation() to get a location object.

You can access pathname,state(can be object string number json array etc) etc.

Example:


const location = useLocation();
console.log(location.pathname); // e.g., '/admin'
Use cases:

Show active links

Conditionally render content based on route

Read state passed via navigate('/path', { state: {...} })
```

# Form Handling using disk and cloudinary (FHDC)
- const multer=require('multer)
- const cloudinary = require('cloudinary').v2; 
###  (FHDC) form 
 - multiple keyword allow multiple file to send at once
 - encType="multipart/form-data"> it is must for multer so that multer can parse it express.json can oarse text only
```
<input type="file" multiple onChange={change} />
```

```
    <form action="http://localhost:2030/sendPic"
      method="POST"
      encType="multipart/form-data">
      <input type="file" multiple  />
      <button type="submit">Upload</button>
    </form>
```
### (FHDC) file contatin 
 ```
  //   console.log(file)
        //   fieldname: 'images', namefiled of input tag
        //   originalname: 'heroImg.jpg',
        //   encoding: '7bit',
        //   mimetype: 'image/jpeg'

```
###  (FHDC) Error Handling
```
// error handling of multer
app.use((err, req, res, next) => {
    if (err) {
        return res.json({ error: err.message });
    }
    next();
});
```

###  (FHDC) filter image ,document or archieves using this only
-     req.file.mimetype.startWith('application/') for pdf,msword,pdf,ppt  
-     req.file.mimetype.startWith('video/') for video
-     req.file.mimetype.startWith('image/') for image
```
const fileFilter = (req, file, cb) => {
  const allowedTypes = [
    // Images
    'image/jpeg',
    'image/png',
    'image/webp',
    'image/gif',
    req.file.mimetype.startWith('image/')

    // Video
    video/mp4
    video/webm
    video/ogg
    video/quicktime
    video/x-msvideo
    video/x-matroska
     req.file.mimetype.startWith('video/')

    // Documents
    'application/pdf',
    'application/msword',
    'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
    'application/vnd.ms-excel',
    'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet',
    'application/vnd.ms-powerpoint',
    'application/vnd.openxmlformats-officedocument.presentationml.presentation',
      // Archives
    'application/zip',
    'application/vnd.rar'
     req.file.mimetype.startWith('application/')
    
    'text/plain',

  
  ];

  if (allowedTypes.includes(file.mimetype)) {
    cb(null, true);
  } else {
    cb(new Error('Only standard image, document, or archive files are allowed!'), false);
  }
};

```

###  (FHDC)way-1  storing in disc
- to store in ram use memorystorage
- to store in server folder usinf disk stroage 
- use req,file,cb
- const upload = multer({ storage, fileFilter }); // we use filter wiith storage
- upload.array('images', 10)passed as middleware where 10 is max file
- diskstorage take destination and filename 
```

// storing in server
const storage = multer.diskStorage({
    destination: function (req, file, cb) {
        cb(null, './uploads'); // 👈 folder where file is saved (create manually or ensure it exists)
    },
    filename: function (req, file, cb) {
        cb(null, Date.now() + file.originalname +path.extname(file.originalname)); // unique file name
    }
});

const upload = multer({ storage, fileFilter }); // we use filter wiith storage
```
### (FHDC)passing file in routes 
- req.files
- req.files.length
```
app.post('/sendPic', upload.array('images', 10), (req, res) => {  // here images is namefiled of input tag must be same
    if (!req.files || req.files.length === 0) {
        return res.json({ error: 'No files selected!' });
    }

    const filePaths = req.files.map((file) => file.path);
    res.json({ message: 'Files uploaded', files: filePaths });
});

```
### (FHDC)way-2 storing file in cloudinary using cloudinary 
> storage(memoryStroage) -> upload -> upload.array (middleware in route) -> uploadtocloudinary(function created by user and return promise with public_id and secure_url with     cloudinary.uploader.upload_stream({ resource_type: 'image' }, (err, result)))
- resource_type: 'image'
- resource_type: 'video' and 'audio'
- resource_type: 'raw' for pdf msword ppt excell etc
- resolve({url: result.secure_url, id: result.public_id }); url for view image and if for delete
- cloudinary.uploader.upload_stream it is used to send image from memory directly just choose from computer and upload 
- file.buffer contain the in-memory image video etc
- function uploadToCloudinary(buffer) return promise
- end(buffer) will tell cloudinary server that file is send now you can upload else cloudinary wait for when image will be sent to their server and when cloudinary will host the image 
```
// ✅ Cloudinary config
cloudinary.config({
  cloud_name: '',
  api_key: '',
  api_secret: ''
});

// ✅ Multer memory storage for file buffers
const storage = multer.memoryStorage();
const upload = multer({ storage });

// ✅ Cloudinary upload function (single buffer)
function uploadToCloudinary(buffer) {
  return new Promise((resolve, reject) => {
    cloudinary.uploader.upload_stream({ resource_type: 'image' }, (err, result) => {
      if (err) return reject(err);
      resolve({
        url: result.secure_url, 
        id: result.public_id
      });
    }).end(buffer);
  });
}

// ✅ Route for multiple image upload
app.post('/sendPic', upload.array('images', 10), async (req, res) => {
  try {
    if (!req.files || req.files.length === 0) {
      return res.status(400).json({ error: 'No files uploaded' });
    }

    const uploadedImages = [];

    for (const file of req.files) {
      const result = await uploadToCloudinary(file.buffer);
      uploadedImages.push(result);
    }

    res.json({ message: 'All images uploaded to Cloudinary', images: uploadedImages });

  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

```
### (FHDC)Deleting file from cloudinary
- public id is unique id used to delete fiel rrom cloudinary
```
 cloudinary.uploader.destroy(PUBLIC_ID,{ resource_type: 'video' }, (error, result) => {
        if (error) {
            // Handle error (not found, credentials, etc.)
            console.error('Delete failed', error);
        } else {
            console.log('Delete result:', result);
        }
    });
```