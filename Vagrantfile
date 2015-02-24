ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'

# BUILD ALL WITH: vagrant up --no-parallel

Vagrant.configure("2") do |config|

  config.vm.define "app" do |v|

    v.vm.synced_folder ".", "/opt/app", type: "rsync",
      rsync__exclude: get_ignored_files()

    v.vm.provider "docker" do |d|
      d.vagrant_machine = "zg-docker-host"
      d.vagrant_vagrantfile = "#{ENV['HOME']}/Workspace/docker/vagrant/Vagrantfile"
      d.build_dir = "."
      d.build_args = ['--tag="zehnergroup/site"']
      d.env = { :NODE_ENV => 'dev' }
      d.remains_running = true
      d.ports = ["8001:8000", "8081:8080", "5859:5858"]
      d.cmd = ["./bin/dev.sh"]
    end
  end

end

def get_ignored_files()
  ignore_file   = ".rsyncignore"
  ignore_array  = []

  if File.exists? ignore_file and File.readable? ignore_file
    File.read(ignore_file).each_line do |line|
      ignore_array << line.chomp
    end
  end

  return ignore_array
end
