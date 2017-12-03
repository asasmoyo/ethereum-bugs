base_dir = __dir__

namespace '1154' do
  bugno = '1154'
  bug_dir = File.join(base_dir, bugno)
  bug_wd = File.join(bug_dir, 'workdir')

  desc 'Preparing things'
  task :prepare do
    sh "mkdir -p #{bug_wd}"
    sh "rm -rf #{bug_wd}/ipc"
    sh "mkdir -p #{bug_wd}/ipc"
    sh "cp #{bug_dir}/genesis.json #{bug_wd}"
  end

  desc 'Run node1, should run prepare first'
  task :node1 do
    sh "rm -rf #{bug_wd}/1"
    sh "mkdir #{bug_wd}/1"
    sh "cp #{bug_dir}/static-nodes.json.1 #{bug_wd}/1/static-nodes.json"

    node1_app = File.join(bug_dir, 'node1')
    Dir.chdir(node1_app) do
      sh 'make'
      sh 'mv build/bin/geth ../workdir/geth1'
    end

    Dir.chdir(bug_wd) do
      sh './geth1 --datadir=1 --networkid=9999 --nodiscover --verbosity=6 \
        --port=9001 --nat="extip:127.0.0.1" \
        --nodekeyhex="49c1afa125b4475bc9f46bdf6e97168c9b6cb2e79b9dac04bcb48925ec34d9f9" \
        console'
    end
  end

  desc 'Run node2, should run prepare first'
  task :node2 do
    sh "rm -rf #{bug_wd}/2"
    sh "mkdir #{bug_wd}/2"
    sh "cp #{bug_dir}/static-nodes.json.2 #{bug_wd}/2/static-nodes.json"

    node2_app = File.join(bug_dir, 'node2')
    Dir.chdir(node2_app) do
      sh 'make'
      sh 'mv build/bin/geth ../workdir/geth2'
    end

    Dir.chdir(bug_wd) do
      sh './geth2 --datadir=2 --networkid=9999 --nodiscover --verbosity=6 \
      --mine --minerthreads=1 --etherbase=0x281055afc982d96fab65b3a49cac8b878184cb16 \
      --port=9002 --nat="extip:127.0.0.1" \
      --nodekeyhex="a7048a044fd33a30836927fef626677e0eb59ffa7024f0075bb735f365731a6d"'
    end
  end
end

namespace '2305' do
  bugno = '2305'
  bug_dir = File.join(base_dir, bugno)
  bug_wd = File.join(bug_dir, 'workdir')

  desc 'Preparing things'
  task :prepare do
      sh "mkdir -p #{bug_wd}"
      sh "rm -rf #{bug_wd}/ipc"
      sh "mkdir -p #{bug_wd}/ipc"
      sh "cp #{bug_dir}/genesis.json #{bug_wd}"
  end

  desc 'Run node1, should run prepare first'
  task :node1 do
    sh "rm -rf #{bug_wd}/1"
    sh "mkdir #{bug_wd}/1"
    sh "cp #{bug_dir}/static-nodes.json.1 #{bug_wd}/1/static-nodes.json"

    node1_app = File.join(bug_dir, 'node1')
    Dir.chdir(node1_app) do
      sh 'make'
      sh 'mv build/bin/geth ../workdir/geth1'
    end

    Dir.chdir(bug_wd) do
      sh './geth1 --datadir=1 --networkid=9999 --nodiscover --genesis=genesis.json --verbosity=6 \
        --port=9001 --nat="extip:127.0.0.1" \
        --nodekeyhex="49c1afa125b4475bc9f46bdf6e97168c9b6cb2e79b9dac04bcb48925ec34d9f9" \
        console'
    end
  end

  desc 'Run node2, should run prepare first'
  task :node2 do
    sh "rm -rf #{bug_wd}/2"
    sh "mkdir #{bug_wd}/2"
    sh "cp #{bug_dir}/static-nodes.json.2 #{bug_wd}/2/static-nodes.json"

    node2_app = File.join(bug_dir, 'node2')
    Dir.chdir(node2_app) do
      sh 'make'
      sh 'mv build/bin/geth ../workdir/geth2'
    end

    Dir.chdir(bug_wd) do
      sh './geth2 --datadir=2 --networkid=9999 --nodiscover --genesis=genesis.json --verbosity=6 \
      --mine --minerthreads=1 --etherbase=0x281055afc982d96fab65b3a49cac8b878184cb16 \
      --port=9002 --nat="extip:127.0.0.1" \
      --nodekeyhex="a7048a044fd33a30836927fef626677e0eb59ffa7024f0075bb735f365731a6d"'
    end
  end
end
