# Group-Project

# Introduction
During the last two weeks at the tech academy I worked with my peers on a team to continue to develop a full MVC / MVVM web application using C# and Entity Frameworks. Working on a legacy code base was a great learning opportunity for fixing bugs, troubleshooting problems, cleaning up code, and adding requested features. We worked on different front end and back end tasks individually while checking in with daily stand ups and daily summary emails to the project manager. I saw how a good developer works with what they have to make a quality product. I worked on several back end stories that I am quite proud of. Because much of the site had already been built, there were also a good deal of front end stories with UI tweaks and improvements that needed to be completed, all of varying degrees of difficulty. Everyone on the team had a chance to work on front end and back end stories. Over the two week sprint I also had the opportunity to work on some other project management and team programming skills that I'm confident I will use time and time again on future projects. 

The following are descriptions of stories I worked on over the course of the project along with some code examples  

# Back End Stories

## CRUD Access

The dashboard currently pulled information from several different views to show information such as company news, jobs, and schedules. They were currently showing links that allowed for editing or deleting information that should only show up for users in the Admin role. I needed to make it so only people with the proper authorization were able to have CRUD functionality
```
 @if (User.IsInRole("Admin"))
        {
            <td>
                @Html.ActionLink("Edit", "Edit", new { id = item.DateStamp }) |
                @Html.ActionLink("Details", "Details", new { id = item.DateStamp }) |
                @Html.ActionLink("Delete", "Delete", new { id = item.DateStamp })
            </td>
        }
        else
        {
            <td>
               
                @Html.ActionLink("Details", "Details", new { id = item.DateStamp })
            </td>
        }
```
## Suspension Check

I needed to add a check to the log in to see if a user had been suspended and redirect them to the proper page
so I used this code to implement that
```
//selects user id to check suspended status
            var user = UserManager.Users.Where(u => u.UserName == userName).SingleOrDefault();

            if (user == null)
            {
                ModelState.AddModelError("", "Invalid login attempt.");
                return View(model);
            }

            if (user.Suspended)
            {
                return View("Lockout");
            }
   ```         
# Front End Stories

## Manager Pop Up

There needed to be a pop up to confirm any changes made or before any data was deleted by an admin. I added an on click event in Jquery. 
```
  <input type="submit" value="Submit" onclick="return confirm('Click ok to change category')" />
  ```
## Password Requirement Check

When a new user registered we want there to be a little window pop up with the requirements for their password. The text of the requirements would be in a red text and would change to green as the requirements were met. This required creating a special div using HTML with the requirements, special styling for before and after the requirements were meet with CSS, and the logic to check the password input on keypress in Jquery
```
    <div class="form-group">
        <div id="pswd_info">
            <ul>
                <lh>Password must meet the following requirements:</lh>

                <li id="letter" class="invalid">At least <strong>one letter</strong></li>
                <li id="capital" class="invalid">At least <strong>one capital letter</strong></li>
                <li id="number" class="invalid">At least <strong>one number</strong></li>
                <li id="length" class="invalid">Be at least <strong>8 characters</strong></li>
                <li id="specialCharacter" class="invalid">Have at least 1 <strong>special character</strong></li>
            </ul>
        </div>
    </div>
    
     }
     
/*===============================
    Styling for the password requirement box
*/
ul, li {
    margin: 0;
    padding: 0;
    list-style-type: none;
    text-align: start; 
    font-size: small;
}

#pswd_info {
    position: absolute;
    bottom: -75px;
    bottom: -115px\9; /* IE Specific */
    right: 55px;
    width: 250px;
    padding: 15px;
    background: #fefefe;
    font-size: .875em;
    border-radius: 5px;
    box-shadow: 0 1px 3px #ccc;
    border: 1px solid #ddd; 
    display:none;
}

#pswd_info h4 {
    margin: 0 0 10px 0;
    padding: 0;
    font-weight: normal;
 }

#pswd_info::before {
    content: "\25B2";
    position: absolute;
    top: -12px;
    left: 45%;
    font-size: 14px;
    line-height: 14px;
    color: #ddd;
    text-shadow: none;
    display: block;
 }

.invalid {
    padding-left: 22px;
    line-height: 24px;
    color: #ec3f41;
}

.valid {
    padding-left: 22px;
    line-height: 24px;
    color: #3a7d34;
}

//password checker
$(window).on('load', function () {
    $("#passwordInput").keyup(function () {
        // set password variable
        var pswd = $(this).val();
        var pattern = new RegExp(/[~`!#$%\^&*+=\-\[\]\\';,/{}|\\":<>\?@()]/);
        //validate the length
        if (pswd.length < 8) {
            $('#length').removeClass('valid').addClass('invalid');
        } else {
            $('#length').removeClass('invalid').addClass('valid');
        }
        //validate letter
        if (pswd.match(/[A-z]/)) {
            $('#letter').removeClass('invalid').addClass('valid');
        } else {
            $('#letter').removeClass('valid').addClass('invalid');
        }

        //validate capital letter
        if (pswd.match(/[A-Z]/)) {
            $('#capital').removeClass('invalid').addClass('valid');
        } else {
            $('#capital').removeClass('valid').addClass('invalid');
        }

        //validate number
        if (pswd.match(/\d/)) {
            $('#number').removeClass('invalid').addClass('valid');
        } else {
            $('#number').removeClass('valid').addClass('invalid');
        }

        //validate special character
        if (pattern.test(pswd)) {
            $('#specialCharacter').removeClass('invalid').addClass('valid');
        }
        else {
            $('#specialCharacter').removeClass('valid').addClass('invalid');
        }
    });
});

$(window).on('load', function () {
    $("#passwordInput").focus(function () {
        $('#pswd_info').show();
    });
});

$(window).on('load', function () {
    $("#passwordInput").blur(function () {
        $('#pswd_info').hide();
    });
});
```
