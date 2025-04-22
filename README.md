# Ansible Role: RustDesk Install 

Installs the RustDesk remote desktop software components (Server and/or Client) on Debian/Ubuntu-based systems.

This role can perform the following actions depending on configuration:

*   **Server Installation:**
    *   Downloads and executes an external installation script (`install.sh` from `techahold/rustdeskinstall`).
    *   Uses the `expect` module to automate the script's prompts (choosing auto WAN IP and skipping HTTP server setup).
    *   Extracts the generated RustDesk **public key** from the script's output.
    *   Saves the extracted public key to `~/rustdesk_public_key.txt` in the home directory of the user running the playbook (`ansible_user`).
*   **Client Installation:**
    *   Downloads a specific version (`1.3.8`) of the RustDesk client `.deb` package from GitHub releases.
    *   Installs the downloaded `.deb` package using `apt`.

## Requirements

*   **Target OS:** Debian / Ubuntu (uses `apt` and expects `.deb` packages).
*   **Root Access:** Requires `become: yes` as the role installs packages and runs the server install script with elevated privileges.
*   **Internet Access:** Required to download the installation script and the client `.deb` package.
*   **Python `pexpect`:** The `ansible.builtin.expect` module requires the `pexpect` Python library on the target machine (`pip install pexpect` or via system packages like `python3-pexpect`). Ansible usually handles this via `apt` dependencies if possible, but it's good to be aware of.
*   **Ansible Version:** 2.9 or higher recommended.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

| Variable         | Required | Default | Type    | Description                                                                                                |
| ---------------- | -------- | ------- | ------- | ---------------------------------------------------------------------------------------------------------- |
| `install_server` | No       | `true`  | Boolean | If `true`, executes the tasks to install the RustDesk server component and extract/save its public key.    |
| `install_client` | No       | `true`  | Boolean | If `true`, executes the tasks to download and install the RustDesk client component (version `1.3.8`). |

## Dependencies

None. (Requires `pexpect` on the target, as noted in Requirements).

## Example Playbook

```yaml
---
- hosts: remote_desktops
  become: yes
  vars:
    # --- Optional Overrides ---
    # Only install the server, not the client
    # install_server: true
    # install_client: false

    # Only install the client, not the server
    # install_server: false
    # install_client: true

  roles:
    # Assuming the role is named 'ansible-role-rustdesk'
    # and located in the standard roles path
    - ansible-role-rustdesk


## License

GPL-3.0

## Author Information

Thorina Boenke (https://www.ait.ac.at)
