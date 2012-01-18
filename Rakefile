# https://github.com/holman/dotfiles
require 'rake'

desc "Dotfiles symlinking"

task :install do
	linkables = Dir.glob('*/**{.symlink}')

	skip_all = false
	overwrite_all = false
	backup_all = false
	zsh_only = false

	puts "[O]verwrite All? [B]ackup All? [Z]sh Only? [S]kip All?"
	case STDIN.gets.chomp
	when 'O' then overwrite_all = true
	when 'B' then backup_all = true
	when 'Z' then zsh_only = true
	when 'S' then skip_all = true
	end


	linkables.each do |linkable|
		unless zsh_only
			overwrite = false
			backup = false

			file = linkable.split('/').last.split('.symlink').last
			target = "#{ENV["HOME"]}/.#{file}"

			if File.exists?(target) || File.symlink?(target)
				unless skip_all || overwrite_all || backup_all
					puts "File already exists: #{target}, what do you want to do? [s]kip, [S]kip all, [o]verwrite, [O]verwrite all, [b]ackup, [B]ackup all"
					case STDIN.gets.chomp
					when 'o' then overwrite = true
					when 'b' then backup = true
					when 'O' then overwrite_all = true
					when 'B' then backup_all = true
					when 'S' then skip_all = true
					when 's' then next
					end
				end
				FileUtils.rm_rf(target) if overwrite || overwrite_all
				`mv "$HOME/.#{file}" "$HOME/.#{file}.backup"` if backup || backup_all
			end
			`ln -s "$PWD/#{linkable}" "#{target}"`
		end
	end


	if zsh_only
		file = Dir.glob('zsh/zshrc.symlink')[0].split('/').last.split('symlink').last
		target = "#{ENV["HOME"]}/.#{file}"

		if File.exists?(target) || File.symlink?(target)
			unless skip_all || overwrite_all || backup_all
				puts ".zshrc already exists... [o]verwrite?"
				case STDIN.gets.chomp
				when 'o' then overwrite = true
				end
			end
			FileUtils.rm_rf(target) if overwrite || overwrite_all
			`mv "$HOME/.#{file}" "$HOME/.#{file}.backup"` if backup_all
		end
		`ln -s "$PWD/zsh/zshrc.symlink" "#{target}"`
	end
end

task :default => :install