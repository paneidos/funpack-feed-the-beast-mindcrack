task :default => :start

$build_dir = File.expand_path("~/funpacks/ftb-mindcrack/build")
$cache_dir = File.expand_path("~/funpacks/ftb-mindcrack/cache")
$working_dir = File.expand_path(ENV['WORKING'] || "~/funpacks/ftb-mindcrack/working")

task :clean do
  system "rm -rf #{$working_dir}"
end

task :start do
  system %Q{
    mkdir -p #{$working_dir}
  }

  gemfile = File.expand_path("#{$build_dir}/Gemfile")
  run = File.expand_path("#{$build_dir}/bin/run")
  Dir.chdir($working_dir) do
    File.write "data.json", <<-EOS
      {
        "name": "Feed it!",
        "access": { "whitelist": [] },
        "settings": {
          "ftb_version": "8.3.2",
          "ftb_pack": "mindcrack",
          "max-players": "22",
          "blacklist": "atnan",
          "gamemode": 2,
          "ops": "whatupdave\\nchrislloyd\\npaneidos",
          "seed": "s33d",
          "allow-nether": true,
          "allow-flight": false,
          "spawn-animals": true,
          "spawn-monsters": false,
          "spawn-npcs": false,
          "whitelist": "whatupdave\\nchrislloyd\\npaneidos"
        }
      }
    EOS

    raise "error" unless system "PORT=4032 RAM=1024 BUNDLE_GEMFILE=#{gemfile} DATAFILE=#{$working_dir}/data.json #{run} 2>&1"
  end
end

task :compile do
  fail unless system "rm -rf #{$build_dir} && mkdir -p #{$build_dir} #{$cache_dir}"
  fail unless system "bin/compile #{$build_dir} #{$cache_dir} 2>&1"
  Dir.chdir($build_dir) do
    if !system("bundle check")
      fail unless system "bundle install --deployment 2>&1"
    end
  end
end

task :import do
  fail unless system "cd #{$working_dir} && #{$build_dir}/bin/import"
end
