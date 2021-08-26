Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/focal64'

  config.vm.hostname = 'ingage-ishiyu'

  config.vm.network :private_network, ip: '192.168.33.10'

  config.vm.provider :virtualbox do |vb|
    vb.gui = false
    vb.cpus = 4
    vb.memory = 6144
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'off']
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'off']
  end

  config.disksize.size = '40GB'
  config.mutagen.orchestrate = true

  config.vm.synced_folder './share', '/home/vagrant/share', type: "rsync",
    rsync_auto: true,
    # ./share/*node_modules/ が除外される
    # refs: https://qiita.com/suin/items/5de842c6bb9fa7846b63
    rsync__exclude: [
      '.idea/',
      '.idea_modules/',
      'node_modules/',
      'coverage/',
      'vendor/',
      # 除外するとフォルダごと消えるのでファイル指定する
      # (ログ生成用のフォルダは欲しい)
      'log/development.log',
      'log/test.log',
      'log/sidekiq.log',
    ]

  config.vm.provision :docker, run: 'always'
  config.vm.provision :docker_compose
end
