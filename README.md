# README

run commands on containers:
docker-compose run --rm app bundle exec rails db:migrate

docker-compose up
email:
http://localhost:1080

Versionando a API
—————–
1 – Dentro da pasta controllers crie uma pasta chamada api
2 – Dentro da pasta criada crie uma nova pasta chamada v1
3 – Dentro da pasta criada crie um arquivo chamado api_controller.rb e coloque nele:

Habilitando o CORS
——————-
1 – Crie um arquivo (ou atualize) chamado cors.rb em config/initializers/ e coloque nele:
Rails.application.config.middleware.insert_before 0, Rack::Cors do
   allow do
     origins '*'
     resource '*',
       headers: :any,
       expose: ['access-token', 'expiry', 'token-type', 'uid', 'client'],
       methods: [:get, :post, :put, :patch, :delete, :options, :head]
   end
end

Configurando o Rack Attack
————————–
1 – Acrescente em config/application:
config.middleware.use Rack::Attack

2 – Crie um arquivo chamado rack_attack.rb no seu config/initializers/ e coloque nele:
class Rack::Attack
   Rack::Attack.cache.store = ActiveSupport::Cache::MemoryStore.new
 
   # Allow all local traffic
   safelist('allow-localhost') do |req|
      '127.0.0.1' == req.ip || '::1' == req.ip
   end
 
   # Allow an IP address to make 5 requests every 5 seconds
   throttle('req/ip', limit: 5, period: 5) do |req|
      req.ip
   end
end

