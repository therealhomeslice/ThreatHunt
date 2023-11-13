#################################################################
import subprocess
import json
 
def get_local_users():
    # Define the PowerShell command to get local users from the workstation
    ps_script = r'''
        $users = Get-LocalUser | Select-Object Name, Enabled, LastLogon
        $users | ConvertTo-Json
    '''
 
    try:
        # Call PowerShell and execute the script
        output = subprocess.check_output(['powershell', '-Command', ps_script], text=True)
 
        # Parse the JSON output to extract local users
        local_users = json.loads(output)
 
        return local_users
 
    except subprocess.CalledProcessError as e:
        print("Error executing PowerShell script:", e)
        return None
 
 
if __name__ == "__main__":
    local_users = get_local_users()
 
    if local_users:
        print("Local Users:")
        for user in local_users:
            print(f"Name: {user['Name']}, Enabled: {user['Enabled']}, Last Logon: {user['LastLogon']}")
    else:
        print("Failed to retrieve local users.")
#####################################################################
