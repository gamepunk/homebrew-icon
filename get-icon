#!/usr/bin/env ruby
# frozen_string_literal: true

require 'net/https'
require 'open-uri'
require 'json'

def icon(app)

  url = URI('https://itunes.apple.com/search?')
  parmas = { term: app,
             country: 'us',
             media: 'software',
             entity: 'software',
             limit: '10' }

  response = Net::HTTP.post_form(url, parmas)

  json = JSON.parse(response.body)

  count = json['resultCount']

  if count.zero?
    puts '查无此项'
    return
  end

  json['results'].each_with_index do |item, index|
    puts "#{index} : #{item['trackCensoredName']}"
  end

  print "请输入要查找的项目(0 到 #{count - 1}): "
  index = $stdin.gets.chomp
  json = json['results'][index.to_i]

  puts "当前选中的是 : #{json['trackCensoredName']}"

  sizes = %w[60x60 100x100 512x512]
  sizes.each_with_index do |size, index|
    puts "#{index} : #{size}"
  end

  print '请选择要下载的大小(0 到 2): '
  index = $stdin.gets
  link = json["artworkUrl#{sizes[index.to_i].split('x')[0]}"]

  url = URI(link)
  URI.open(url) do |image|
    f = File.new("#{Dir.home}/Desktop/#{json["trackCensoredName"]} - #{sizes[index.to_i]}.jpg", 'w')
    f.write(image.read)
    f.close
    puts '下载完成'
  end
end

if ARGV.count >= 0
  app = ARGV[0]
  puts "请输入要查找的APP: #{app}"
else
  puts '请输入要查找的APP: '
  app = $stdin.gets.chomp
end

icon app
