function get_vm_list{
    param (
        $offset = 0
    )

    # Define the payload for the API request
    $body = @{
        "kind"           =  "vm"
        "sort_order"     =  "ASCENDING"
        "sort_attribute" =  "vmName"
        "length"         =  500
        "filter"         =  "power_state == on"
        "offset"         =  $offset
        "filter"         =  ""
    }

    # Send the API request to retrieve the VM list
    $api_response=Invoke-WebRequest  -authentication Basic  -Uri "$($url)vms/list" -Method POST -credential $api_cred -ContentType "application/json" -Body ($body | ConvertTo-Json) 
    $json=$api_response.Content | ConvertFrom-Json

    # Parse the response JSON to extract the list of VMs
    $vm_list = $json.entities
    # Check if there are more VMs to retrieve
    if ($json.metadata.total_matches -gt ($offset + 500)){
        # Recursively retrieve the next page of VMs and concatenate the results
        $vm_list += get_vm_list -offset ($offset + 500)
    }
    # Return the list of VMs with their UUID, name and other useful informations
    return $vm_list
}
