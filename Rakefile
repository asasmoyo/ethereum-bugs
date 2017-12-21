require 'json'

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

  desc 'Run node0, should run prepare first'
  task :node0 do
    sh "rm -rf #{bug_wd}/0"
    sh "mkdir #{bug_wd}/0"
    sh "cp #{bug_dir}/static-nodes.json.0 #{bug_wd}/0/static-nodes.json"

    node_dir = File.join(bug_dir, 'node0')
    Dir.chdir(node_dir) do
      sh 'make'
      sh 'mv build/bin/geth ../workdir/geth0'
    end

    Dir.chdir(bug_wd) do
      sh './geth0 --datadir=0 --networkid=9999 --nodiscover --genesis=genesis.json --verbosity=6 \
        --port=9000 --nat="extip:127.0.0.1" \
        --nodekeyhex="49c1afa125b4475bc9f46bdf6e97168c9b6cb2e79b9dac04bcb48925ec34d9f9" \
        --mine --minerthreads=1 --etherbase=0x281055afc982d96fab65b3a49cac8b878184cb16'
    end
  end

  desc 'Run node1, should run prepare first'
  task :node1 do
    sh "rm -rf #{bug_wd}/1"
    sh "mkdir #{bug_wd}/1"
    sh "cp #{bug_dir}/static-nodes.json.1 #{bug_wd}/1/static-nodes.json"

    node_dir = File.join(bug_dir, 'node1')
    Dir.chdir(node_dir) do
      sh 'make'
      sh 'mv build/bin/geth ../workdir/geth1'
    end

    Dir.chdir(bug_wd) do
      sh './geth1 --datadir=1 --networkid=9999 --nodiscover --genesis=genesis.json --verbosity=6 \
      --port=9001 --nat="extip:127.0.0.1" \
      --nodekeyhex="a7048a044fd33a30836927fef626677e0eb59ffa7024f0075bb735f365731a6d"'
    end
  end

  desc 'Run node2, should run prepare first'
  task :node2 do
    sh "rm -rf #{bug_wd}/2"
    sh "mkdir #{bug_wd}/2"
    sh "cp #{bug_dir}/static-nodes.json.2 #{bug_wd}/2/static-nodes.json"

    node_dir = File.join(bug_dir, 'node2')
    Dir.chdir(node_dir) do
      sh 'make'
      sh 'mv build/bin/geth ../workdir/geth2'
    end

    Dir.chdir(bug_wd) do
      sh './geth2 --datadir=2 --networkid=9999 --nodiscover --genesis=genesis.json --verbosity=6 \
      --port=9002 --nat="extip:127.0.0.1" \
      --nodekeyhex="075f47f24e5a94949f2c2fe273b444d884959f89a04727bf96bb2e0b94f345cb"'
    end
  end

  namespace :samc do
    desc 'Prepare cluster for samc. It needs the number of nodes and cluster dir'
    task :setup, [:num_nodes, :cluster_dir] do |task, args|
      static_nodes = {}

      puts "> Initializing ethereum cluster at #{args[:cluster_dir]} with #{args[:num_nodes]} nodes"

      # create cluster_dir
      puts '> Create cluster dir'
      sh "mkdir -p #{args[:cluster_dir]}"

      # create each node workdir
      puts '> Create each node workdir'
      for i in 0..args[:num_nodes].to_i-1 do
        sh "mkdir -p #{args[:cluster_dir]}/#{i}"
      end

      # copy genesis block
      puts '> Copy genesis block'
      for i in 0..args[:num_nodes].to_i-1 do
        sh "cp #{bug_dir}/genesis.json #{args[:cluster_dir]}/#{i}"
      end

      # create nodekey
      puts '> Create nodekey'
      for i in 0..args[:num_nodes].to_i-1 do
        enode = create_nodekey("#{args[:cluster_dir]}/#{i}")
        static_nodes[i] = "enode://#{enode}@127.0.0.1:#{9000+i}"
      end

      # create static-nodes.json
      puts '> Create static-nodes.json'
      for i in 0..args[:num_nodes].to_i-1 do
        curr_static_nodes = []
        static_nodes.each do |k, v|
          next if k == i

          curr_static_nodes.push(v)
        end

        static_nodes_path = File.join(args[:cluster_dir], i.to_s, 'static-nodes.json')
        File.open(static_nodes_path, 'w') do |f|
          f.puts JSON.generate(curr_static_nodes)
        end
      end
    end

    desc 'Start the node id in cluster dir in background'
    task :start, [:node_id, :cluster_dir] do |task, args|
      puts "Starting node #{args[:node_id]}"

      # delete unused files
      unused_files = [
        'chaindata',
        'dapp',
        'geth.ipc',
        'nodes',
        'pid',
        'geth',
        'log',
      ]
      unused_files.each do |v|
        sh "rm -rf #{args[:cluster_dir]}/#{args[:node_id]}/#{v}"
      end

      # compile and copy binary
      compile_eth(
        File.join(bug_dir, "node#{args[:node_id].to_i}"),
        File.join(args[:cluster_dir], args[:node_id].to_s)
      )

      # start the node
      nodekey = get_nodekey(args[:cluster_dir], args[:node_id])
      command = "./geth --datadir=. --networkid=9999 --nodiscover --genesis=genesis.json --verbosity=6 \
      --port=#{9000+args[:node_id].to_i} --nat=\"extip:127.0.0.1\" \
      --nodekeyhex=#{nodekey}"

      # run miner if it is node0
      command += ' --mine --minerthreads=1 --etherbase=0x281055afc982d96fab65b3a49cac8b878184cb16' if args[:node_id].to_i == 0

      pid = run_in_background(
        command,
        File.join(args[:cluster_dir], args[:node_id]),
        File.join(args[:cluster_dir], args[:node_id], 'pid'),
        File.join(args[:cluster_dir], args[:node_id], 'log')
      )
      puts "Got pid #{pid}"
    end

    desc 'Gracefully stop node id in cluster dir'
    task :stop, [:node_id, :cluster_dir] do |task, args|
      puts "Stopping node #{args[:node_id]}"

      pid_file = File.join(args[:cluster_dir], args[:node_id], 'pid')
      stop_process(pid_file)
    end

    desc 'Forcefully stop node id in cluster dir'
    task :kill, [:node_id, :cluster_dir] do |task, args|
      puts "Killing node #{args[:node_id]}"

      pid_file = File.join(args[:cluster_dir], args[:node_id], 'pid')
      force_kill_process(pid_file)
    end
  end
end

def create_nodekey(dir)
  bootnode_bin = File.join(__dir__, 'bootnode')
  sh "#{bootnode_bin} -genkey #{dir}/nodekey"
  return `#{bootnode_bin} -nodekey #{dir}/nodekey -writeaddress`.strip!
end

# compile eth located in dir
# then copy resulting geth binary into to
def compile_eth(dir, to)
  Dir.chdir(dir) do
    sh 'make'
    sh "cp build/bin/geth #{to}/geth"
  end
end

def get_nodekey(cluster_dir, node_id)
  return IO.read(File.join(cluster_dir, node_id.to_s, 'nodekey'))
end

def run_in_background(command, chdir, pid_file_path, log_path)
  pid = Process.spawn(command, :chdir => chdir, [:out, :err] => [log_path, 'w'])
  File.open(pid_file_path, 'w') do |f|
    f.puts pid
  end
  return pid
end

def stop_process(pid_path)
  pid = IO.read pid_path if File.file?(pid_path)
  Process.kill('SIGINT', pid.to_i) unless pid == nil
  File.delete(pid_path) if File.file?(pid_path)
end

def force_kill_process(pid_path)
  pid = IO.read pid_path if File.file?(pid_path)
  Process.kill('SIGKILL', pid.to_i) unless pid == nil
  File.delete(pid_path) if File.file?(pid_path)
end