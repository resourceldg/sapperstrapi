<script>
    import { goto, stores } from "@sapper/app";
    const { session } = stores();
  
    let password = "";
    let confirm_pass = "";
    let placeholderpass = "password";
    let placeholderpassconfirm = "confirm password";
    let placeholderemail = "email";

    let email = "";
    let error;
  
    const handleSignup = async () => {
      
      if (password === confirm_pass){
        const response = await fetch("/signup", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            Accept: "application/json",
          },
          body: JSON.stringify({ email, password }),
        });
    
        const parsed = await response.json();
    
        if (parsed.error) {
          error = parsed.error;
        } else {
          $session.token = parsed.token;
          goto("/about");
        }

      }else{
        placeholderpass="password confirm not match",
        placeholderpassconfirm="please retry passwords"

      }
    };
    

    
  </script>
  
    
    <form on:submit|preventDefault="{handleSignup}" method="post">
      
      <label>
        Email:
        <input type="email" placeholder={placeholderemail} bind:value="{email}" />
      </label>

      <label>
        Password:
        <input id= "pass" name= "password" type="password" placeholder={placeholderpass} bind:value="{password}"/>
      </label>

      <label>
        Confirm Password:
        <input id= "confirm_pass" name="confirm_pass" type="password" placeholder={placeholderpassconfirm} bind:value="{confirm_pass}" />  
      </label>      
      
      <button id='submit' name='submit' type="submit" value= "Registration">login</button>
      
    </form>
    
    
    {#if error}
    <p>{error}</p>
    {/if}