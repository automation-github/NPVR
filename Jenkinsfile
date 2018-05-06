pipeline {
  agent any
  stages {
    stage('installation') {
      steps {
            sh 'python2.7 /var/tmp/Nightswatch/deploy/upgrade_machines_sa.py -p $target_cluster --ininame \'/var/lib/jenkins/nightswatch/5.1/config.ini\''
          }
    }
    stage('npvr_schedule_5.1') {
      agent any
      environment {
        test_name = 'test_npvr_schedule.py'
      }
      steps {
        build(job: 'npvr_schedule_5.1', propagate: false)
      }
    }
    stage('npvr_update_5.1') {
      agent any
      environment {
        test_name =  'test_npvr_update.py'
      }
      steps {
        build(job: 'npvr_update_5.1', propagate: false)
      }
    }
    stage('npvr_delete_5.1') {
      agent any
      environment {
        test_name =  'test_npvr_delete_stop.py'
      }
      steps {
        build(job: 'npvr_delete_5.1', propagate: false)
      }
    }
    stage('npvr_ad_zone_5.1') {
      agent any
      environment {
        test_name =  'test_npvr_ad_zone.py'
      }
      steps {
        build(job: 'npvr_ad_zone_5.1', propagate: false)
      }
    }
    stage('copy xmls') {
      steps {
        sh 'scp -p root@$target_cluster:/tmp/Fragment_cache/*.xml .'
      }
    }
    stage('check_cores') {
      steps {
        sh 'ssh root@$target_cluster "python2.7 /var/Nightswatch/deploy/find_cores.py"'
      }
    }
    stage('publish junit results') {
      steps {
        junit(testResults: '*.xml', healthScaleFactor: 1.0, allowEmptyResults: true)
      }
    }
  }
  environment {
    target_cluster = '10.65.183.143'
  }
}
