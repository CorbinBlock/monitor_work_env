pipeline {
    agent none
    stages {
        stage('check_batch') {
            agent { 
                label 'WIN-03-DELL'
            }
            steps {
                pwsh("""
              sqlcmd -S \$env:a_server_01 -i \$env:a_daily_batch_script;sqlcmd -S \$env:a_server_02 -i \$env:a_daily_batch_script
              # Get-Process java
                """)
            }
        }
        stage('check_pref') {
            agent { 
                label 'WIN-03-DELL'
            }
            steps {
                pwsh("""
              sqlcmd -S \$env:a_server_03 -i \$env:a_pref_monitor_script
              # Get-Process java
                """)
            }
        }
        stage('check_data_dirs') {
            agent { 
                label 'WIN-03-DELL'
            }
            steps {
                pwsh("""
                Get-ChildItem \$env:b_data_dir
                # Get-Process java

                """)
            }
        }
        stage('write_emails_to_disk') {
            agent { 
                label 'WIN-03-DELL'
            }
            steps {
                pwsh"""
                Set-Location \$env:windows_automation_dir
                ./setup_venv.ps1
                ./activate_venv.ps1
                ./upgrade_pip.ps1
                ./install_requirements.ps1
                ./run_read_email.ps1
                Get-Process java
                """
            }
        }
    }
}
