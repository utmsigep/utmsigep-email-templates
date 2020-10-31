#!/usr/bin/env ruby

require 'dotenv'
require 'net/http'
require 'fileutils'
require 'createsend'

Dotenv.load

# Configuration
api_key = ENV['CAMPAIGN_MONITOR_API_KEY']
client_id = ENV['CAMPAIGN_MONITOR_CLIENT_ID']
baseurl = 'https://github.com/utmsigep/utmsigep-email-templates/blob/master'
basepath = Dir.pwd

task default: %w[build]

task :build do
  templates = get_templates(api_key, client_id)
  templates.each do |t|
    Dir.chdir("#{basepath}/#{t[:path]}")
    system("zip -q -r #{t[:path]}-assets.zip images/* style.css")
  end
end

task :update do
  auth = { api_key: api_key }
  cs = CreateSend::CreateSend.new(auth)
  templates = get_templates(api_key, client_id)
  templates.each do |t|
    template = CreateSend::Template.new auth, t[:id]
    template.update(t[:name], "#{baseurl}/#{t[:path]}/index.html?raw=1", "#{baseurl}/#{t[:path]}/#{t[:path]}-assets.zip?raw=1")
  end
end

private

def get_templates(api_key, client_id)
  auth = { api_key: api_key }
  cs = CreateSend::CreateSend.new(auth)
  client = CreateSend::Client.new auth, client_id
  templates = []
  client.templates.each do |t|
    templates.push({
        name: t[:Name],
        id: t[:TemplateID],
        path: t[:Name].downcase.gsub(' ', '-').gsub(/[^0-9a-z\- ]/i, '')
      })
  end
  return templates
end
